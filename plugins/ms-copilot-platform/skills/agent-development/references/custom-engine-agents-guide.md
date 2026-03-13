# Custom Engine Agents — Comprehensive Guide

Custom engine agents provide full control over the AI stack: model selection, orchestration
framework, hosting, tools, and channels. This is the most powerful — and most complex — path
in the Microsoft agent ecosystem.

## When to Choose Custom Engine

Choose this path when:
- A specific AI model is required (not Copilot's built-in model)
- Custom orchestration logic is needed (multi-step reasoning, agent coordination)
- Deployment outside M365 is required (web, SMS, email, customer portals)
- Autonomous actions without user input are needed
- Non-Microsoft AI frameworks are required (LangChain, Crew.ai, etc.)
- Agent-to-agent coordination via A2A protocol is needed
- MCP server hosting or custom tool servers are part of the architecture

## Microsoft 365 Agents SDK

The primary framework for custom engine agents. Available in three languages:

### Language Options

**C# / .NET** — `https://github.com/microsoft/Agents-for-net`
- Best for: Teams-heavy orgs, enterprise .NET shops, Azure Functions hosting
- Mature ecosystem, strong Semantic Kernel integration

**Python** — `https://github.com/microsoft/Agents-for-python`
- Best for: AI/ML teams, LangChain/LangGraph users, rapid prototyping
- Largest AI library ecosystem, fastest iteration

**JavaScript / TypeScript** — `https://github.com/microsoft/Agents-for-js`
- Best for: Web-first teams, Node.js infrastructure, serverless Functions
- Good for full-stack teams already in the JS ecosystem

### Project Structure (Python)

```
my-custom-agent/
├── src/
│   ├── app.py                # Entry point — AgentHost setup
│   ├── my_agent.py           # ActivityHandler — core agent logic
│   ├── config.py             # Configuration and environment vars
│   └── tools/                # Custom tool functions
│       ├── __init__.py
│       ├── crm_tools.py      # CRM integration tools
│       └── email_tools.py    # Email tools via MCP or Graph
├── requirements.txt          # Python dependencies
├── Dockerfile                # Container definition for deployment
├── docker-compose.yml        # Local development with dependencies
├── appPackage/
│   ├── manifest.json         # M365 app manifest
│   ├── outline.png           # App icon (32x32)
│   └── color.png             # App icon (192x192)
├── env/
│   ├── .env.dev              # Development environment vars
│   └── .env.prod             # Production environment vars
├── infra/
│   ├── main.bicep            # Azure infrastructure definition
│   ├── main.parameters.json  # Bicep parameters
│   └── modules/
│       ├── bot-service.bicep
│       ├── app-service.bicep
│       └── keyvault.bicep
├── tests/
│   ├── test_agent.py
│   └── test_tools.py
└── README.md
```

### Core Concepts

#### Activity Handler
The agent's brain loop. Receives messages, processes them, sends responses:

```python
from microsoft_agents.builder import AgentBuilder
from microsoft_agents.hosting.aio import AgentHost

class MyAgent(ActivityHandler):
    def __init__(self, ai_service, tools):
        self.ai_service = ai_service
        self.tools = tools

    async def on_message_activity(self, turn_context):
        user_message = turn_context.activity.text
        # Orchestrate AI response with tools
        response = await self.ai_service.generate(
            message=user_message,
            tools=self.tools,
            history=turn_context.conversation_state
        )
        await turn_context.send_activity(response)

    async def on_members_added_activity(self, members_added, turn_context):
        for member in members_added:
            if member.id != turn_context.activity.recipient.id:
                await turn_context.send_activity("Hello! How can I help?")
```

#### AI Integration — Model Agnostic
The SDK does not lock to any model. Wire up any AI service:

```python
# Azure OpenAI
from openai import AsyncAzureOpenAI
client = AsyncAzureOpenAI(
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
    api_key=os.getenv("AZURE_OPENAI_KEY"),
    api_version="2024-12-01-preview"
)

# Or use Semantic Kernel for orchestration
from semantic_kernel import Kernel
kernel = Kernel()
kernel.add_service(AzureChatCompletion(service_id="chat", ...))
```

## Orchestration Frameworks — Deep Dive

### Semantic Kernel

Microsoft's orchestration framework. Best for M365-integrated agents.

```python
from semantic_kernel import Kernel
from semantic_kernel.functions import kernel_function
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion

# Setup
kernel = Kernel()
kernel.add_service(AzureChatCompletion(
    service_id="chat",
    deployment_name="gpt-4o",
    endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
    api_key=os.getenv("AZURE_OPENAI_KEY")
))

# Define tools as plugins
class CRMPlugin:
    @kernel_function(description="Look up a customer by name or ID")
    async def lookup_customer(self, query: str) -> str:
        result = await crm_api.search(query)
        return json.dumps(result)

    @kernel_function(description="Create a new support ticket")
    async def create_ticket(self, customer_id: str, title: str, priority: str) -> str:
        ticket = await crm_api.create_ticket(customer_id, title, priority)
        return f"Ticket {ticket.id} created"

kernel.add_plugin(CRMPlugin(), "crm")

# Invoke with automatic tool calling
result = await kernel.invoke_prompt(
    "Help the user with their request: {{$input}}",
    input_vars={"input": user_message}
)
```

**Key Semantic Kernel features:**
- Function calling with automatic tool selection
- Prompt templates with variables
- Memory (conversation history, semantic memory)
- Planners (step-by-step execution planning)
- Filters (pre/post function call hooks for logging, auth, etc.)

### LangChain / LangGraph

Open-source framework. Best for complex reasoning chains and agent graphs.

```python
from langchain_openai import AzureChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain.tools import tool
from langchain_core.prompts import ChatPromptTemplate

# Setup
llm = AzureChatOpenAI(
    azure_deployment="gpt-4o",
    api_version="2024-12-01-preview"
)

# Define tools
@tool
def lookup_customer(query: str) -> str:
    """Look up a customer by name or ID in the CRM system."""
    return json.dumps(crm_api.search(query))

@tool
def create_ticket(customer_id: str, title: str, priority: str) -> str:
    """Create a support ticket for a customer."""
    ticket = crm_api.create_ticket(customer_id, title, priority)
    return f"Ticket {ticket.id} created"

# Create agent
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful customer service agent."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

agent = create_tool_calling_agent(llm, [lookup_customer, create_ticket], prompt)
executor = AgentExecutor(agent=agent, tools=[lookup_customer, create_ticket])

# Run
result = await executor.ainvoke({"input": user_message})
```

**LangGraph for complex agent flows:**

```python
from langgraph.graph import StateGraph, START, END

# Define agent states and transitions
graph = StateGraph(AgentState)
graph.add_node("classify", classify_intent)
graph.add_node("lookup", lookup_customer)
graph.add_node("create_ticket", create_ticket)
graph.add_node("respond", generate_response)

graph.add_edge(START, "classify")
graph.add_conditional_edges("classify", route_by_intent)
graph.add_edge("lookup", "respond")
graph.add_edge("create_ticket", "respond")
graph.add_edge("respond", END)

agent_graph = graph.compile()
```

## A2A Protocol — Agent-to-Agent Communication

A2A enables multi-agent systems where specialized agents coordinate:

### Architecture Pattern

```
User → Router Agent → [HR Agent, IT Agent, Finance Agent]
                         ↕ A2A       ↕ A2A       ↕ A2A
                      [HR Tools]  [IT Tools]  [Finance Tools]
```

### How A2A Works

1. **Agent Card** — Each agent publishes a JSON description of its capabilities
2. **Discovery** — Agents discover each other via agent card endpoints
3. **Task Delegation** — A router agent sends tasks to specialized agents
4. **Response Aggregation** — Results flow back through the coordination layer

### Agent Card Example

```json
{
  "name": "HR Benefits Agent",
  "description": "Answers questions about employee benefits, PTO, and enrollment",
  "capabilities": ["benefits_lookup", "pto_balance", "enrollment_status"],
  "endpoint": "https://hr-agent.azurewebsites.net/a2a",
  "authentication": {
    "type": "oauth2",
    "authority": "https://login.microsoftonline.com/{tenant-id}"
  }
}
```

### Implementing A2A

Use the Agents SDK's A2A support or implement the protocol directly:

```python
# A2A Client — calling another agent
from microsoft_agents.a2a import A2AClient

hr_agent = A2AClient(
    endpoint="https://hr-agent.azurewebsites.net/a2a",
    credential=managed_identity_credential
)

# Delegate a task
result = await hr_agent.send_task({
    "type": "query",
    "content": "What is the PTO balance for employee #12345?"
})
```

## MCP Server Integration

Custom engine agents can connect to MCP servers for tool access:

### Microsoft's First-Party MCP Servers
- **Outlook** — Email read/write, calendar, contacts
- **Teams** — Messages, channels, meetings
- **Search** — Microsoft Search across M365
- **Fabric** — Data queries, semantic models
- **D365** — CRM/ERP operations

### Hosting Your Own MCP Server

For custom tools, host an MCP server in Azure:

```python
# mcp_server.py — Custom MCP server using FastMCP
from fastmcp import FastMCP

mcp = FastMCP("my-custom-tools")

@mcp.tool()
def query_inventory(product_id: str) -> str:
    """Check current inventory levels for a product."""
    result = inventory_db.query(product_id)
    return json.dumps(result)

@mcp.tool()
def place_order(product_id: str, quantity: int, customer_id: str) -> str:
    """Place an order for a customer."""
    order = orders_api.create(product_id, quantity, customer_id)
    return json.dumps(order)

if __name__ == "__main__":
    mcp.run(transport="sse", port=8080)
```

**Hosting options for MCP servers:**
- **Azure Container Apps** — Serverless containers, auto-scale
- **Azure App Service** — Traditional web hosting
- **Azure Functions** — Event-driven, pay-per-execution
- **AKS** — Kubernetes for complex multi-server setups

Read `references/infrastructure-requirements.md` for Docker and Azure hosting setup.

## Deployment Patterns

### Local Development
```bash
# With Docker Compose
docker-compose up  # Starts agent + dependencies

# Without Docker
python -m pip install -r requirements.txt
python src/app.py
```

### Azure Deployment via Bicep
```bash
# Deploy infrastructure
az deployment group create \
  --resource-group rg-my-agent \
  --template-file infra/main.bicep \
  --parameters infra/main.parameters.json

# Deploy code
az webapp deploy --resource-group rg-my-agent --name app-my-agent --src-path ./
```

### CI/CD Pipeline
```yaml
# GitHub Actions example
name: Deploy Agent
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: azure/login@v2
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - run: |
          az deployment group create ...
          az webapp deploy ...
```

## Testing

### Unit Tests
Test tool functions independently:
```python
async def test_lookup_customer():
    result = await crm_tools.lookup_customer("Contoso")
    assert "Contoso" in result
```

### Integration Tests
Test the full agent flow with mocked AI:
```python
async def test_agent_handles_ticket_request():
    agent = MyAgent(mock_ai_service, tools)
    context = create_test_context("Create a ticket for Contoso about billing")
    await agent.on_message_activity(context)
    assert "Ticket" in context.sent_activities[0].text
```

### Agents Playground
Local test environment simulating Teams — no M365 tenant needed.

## Common Architecture Patterns

### Pattern 1: Simple Q&A with Tools
Agent + LLM + 2-3 tools. Good starting point for most custom engine agents.

### Pattern 2: RAG Agent
Agent + LLM + Vector Store + Document Retrieval. For knowledge-heavy agents.

### Pattern 3: Workflow Agent
Agent + LLM + State Machine + Multiple Tools + Human-in-the-Loop.
For business process automation with approval steps.

### Pattern 4: Multi-Agent System
Router Agent + Specialist Agents via A2A. For complex domains
where no single agent can handle all scenarios.

### Pattern 5: Autonomous Agent
Agent + Event Triggers + Tools + Scheduling. Proactive agents that
monitor conditions and take action without user input.
