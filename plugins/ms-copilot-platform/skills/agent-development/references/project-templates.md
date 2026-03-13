# Reusable Project Templates

Scaffold templates for each agent type. These templates are used by both the
guided walkthrough and autonomous build modes to ensure consistency.

## Template Index

| Template | Agent Type | Language | Framework |
|----------|-----------|----------|-----------|
| T1 | Custom Engine | Python | Semantic Kernel |
| T2 | Custom Engine | Python | LangChain |
| T3 | Custom Engine | C# / .NET | Semantic Kernel |
| T4 | Custom Engine | TypeScript | Semantic Kernel |
| T5 | Declarative Agent | JSON + API | Agents Toolkit |
| T6 | Copilot Studio | Solution Package | Power Platform |
| T7 | MCP Server | Python | FastMCP |
| T8 | Multi-Agent (A2A) | Python | Mixed |

## T1: Custom Engine — Python + Semantic Kernel

### File Manifest

```
{agent-name}/
├── README.md
├── src/
│   ├── app.py
│   ├── agent.py
│   ├── config.py
│   └── tools/
│       ├── __init__.py
│       └── {domain}_tools.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── appPackage/
│   ├── manifest.json
│   ├── outline.png
│   └── color.png
├── infra/
│   ├── main.bicep
│   ├── main.parameters.json
│   └── modules/
│       ├── app-service.bicep
│       ├── bot-service.bicep
│       ├── keyvault.bicep
│       ├── identity.bicep
│       └── monitoring.bicep
├── scripts/
│   ├── deploy.sh
│   ├── cleanup.sh
│   └── setup-local.sh
├── tests/
│   ├── test_agent.py
│   └── test_tools.py
└── .github/workflows/deploy.yml
```

### app.py (Entry Point)
```python
"""Agent entry point — configures and starts the agent host."""
import os
from dotenv import load_dotenv
from microsoft_agents.builder import AgentBuilder
from microsoft_agents.hosting.aio import AgentHost
from agent import create_agent
from config import Settings

load_dotenv()
settings = Settings()

agent = create_agent(settings)

host = AgentHost()
host.add_agent("agent", agent)
host.run(port=settings.port)
```

### agent.py (Core Agent)
```python
"""Core agent logic — handles messages and orchestrates AI responses."""
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion
from microsoft_agents.core import ActivityHandler
from config import Settings

def create_agent(settings: Settings) -> ActivityHandler:
    # Initialize Semantic Kernel
    kernel = Kernel()
    kernel.add_service(AzureChatCompletion(
        service_id="chat",
        deployment_name=settings.azure_openai_deployment,
        endpoint=settings.azure_openai_endpoint,
        api_key=settings.azure_openai_key
    ))

    # Register tool plugins
    from tools import DomainTools
    kernel.add_plugin(DomainTools(), "domain")

    class Agent(ActivityHandler):
        async def on_message_activity(self, turn_context):
            user_message = turn_context.activity.text
            result = await kernel.invoke_prompt(
                f"{{{{$input}}}}",
                input_vars={"input": user_message}
            )
            await turn_context.send_activity(str(result))

    return Agent()
```

### config.py (Settings)
```python
"""Configuration management — loads from environment variables."""
import os

class Settings:
    def __init__(self):
        self.port = int(os.getenv("PORT", "3978"))
        self.azure_openai_endpoint = os.environ["AZURE_OPENAI_ENDPOINT"]
        self.azure_openai_key = os.environ["AZURE_OPENAI_KEY"]
        self.azure_openai_deployment = os.getenv("AZURE_OPENAI_DEPLOYMENT", "gpt-4o")
        self.app_id = os.environ.get("MICROSOFT_APP_ID", "")
        self.app_password = os.environ.get("MICROSOFT_APP_PASSWORD", "")
```

### .env.example
```
# Azure OpenAI
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_KEY=your-key-here
AZURE_OPENAI_DEPLOYMENT=gpt-4o

# Microsoft App Registration
MICROSOFT_APP_ID=
MICROSOFT_APP_PASSWORD=

# Agent Configuration
PORT=3978
```

## T7: MCP Server — Python + FastMCP

### File Manifest

```
{mcp-server-name}/
├── README.md
├── mcp_server.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── infra/
│   ├── container-app.bicep
│   └── container-app.parameters.json
├── scripts/
│   ├── deploy.sh
│   ├── cleanup.sh
│   └── test-tools.sh
└── tests/
    └── test_tools.py
```

### mcp_server.py
```python
"""MCP Server — exposes domain tools for agent consumption."""
from fastmcp import FastMCP
import os
from dotenv import load_dotenv

load_dotenv()

mcp = FastMCP(
    name="{server-name}",
    description="{Server description}"
)

@mcp.tool()
async def example_tool(param1: str, param2: int = 10) -> dict:
    """Description of what this tool does.

    Args:
        param1: Description of param1
        param2: Description of param2 (default: 10)
    """
    # Implementation here
    return {"result": "success"}

@mcp.resource("{domain}://items")
async def list_items() -> str:
    """List all available items."""
    # Implementation here
    return "[]"

if __name__ == "__main__":
    transport = os.getenv("MCP_TRANSPORT", "sse")
    port = int(os.getenv("MCP_PORT", "8080"))
    mcp.run(transport=transport, host="0.0.0.0", port=port)
```

## T8: Multi-Agent System (A2A)

### File Manifest

```
{system-name}/
├── README.md
├── router/                  # Router/orchestrator agent
│   ├── src/
│   │   ├── app.py
│   │   ├── router_agent.py
│   │   └── agent_registry.py
│   ├── requirements.txt
│   └── Dockerfile
├── agents/                  # Specialist agents
│   ├── {agent-1}/
│   │   ├── src/
│   │   │   ├── app.py
│   │   │   ├── agent.py
│   │   │   └── tools/
│   │   ├── agent-card.json
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   └── {agent-2}/
│       └── ... (same structure)
├── docker-compose.yml       # Runs all agents locally
├── infra/
│   ├── main.bicep
│   └── modules/
│       ├── container-apps-env.bicep
│       ├── router-app.bicep
│       └── agent-app.bicep
├── scripts/
│   ├── deploy-all.sh
│   ├── cleanup-all.sh
│   └── test-a2a.sh
└── .github/workflows/deploy.yml
```

### docker-compose.yml for Multi-Agent
```yaml
version: '3.8'
services:
  router:
    build: ./router
    ports:
      - "3978:3978"
    environment:
      - AGENT_REGISTRY_URL=http://registry:8080
      - AZURE_OPENAI_ENDPOINT=${AZURE_OPENAI_ENDPOINT}
      - AZURE_OPENAI_KEY=${AZURE_OPENAI_KEY}
    depends_on:
      - agent-hr
      - agent-it

  agent-hr:
    build: ./agents/hr-agent
    ports:
      - "3979:3978"
    environment:
      - AZURE_OPENAI_ENDPOINT=${AZURE_OPENAI_ENDPOINT}
      - AZURE_OPENAI_KEY=${AZURE_OPENAI_KEY}

  agent-it:
    build: ./agents/it-agent
    ports:
      - "3980:3978"
    environment:
      - AZURE_OPENAI_ENDPOINT=${AZURE_OPENAI_ENDPOINT}
      - AZURE_OPENAI_KEY=${AZURE_OPENAI_KEY}
```

## Template Customization Notes

When generating from these templates:
1. Replace all `{placeholders}` with actual names
2. Customize tools/ directory for the specific domain
3. Adjust Bicep modules based on actual infrastructure needs
4. Update manifest.json with correct app IDs and descriptions
5. Set appropriate AI model and deployment names
6. Add domain-specific test cases
7. Customize README with architecture-specific content
