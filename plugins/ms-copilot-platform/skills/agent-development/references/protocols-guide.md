# Protocols Guide — A2A, MCP, and OpenAPI

Implementation patterns for the three open protocols in the Microsoft Frontier Agent Platform.

## Protocol Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    AGENT PROTOCOLS                          │
├──────────────┬──────────────────┬───────────────────────────┤
│     A2A      │       MCP        │         OpenAPI           │
│ Agent↔Agent  │    Agent↔Tool    │       Agent↔API           │
│              │                  │                           │
│ Delegation   │ Tool discovery   │ Standard REST calls       │
│ Coordination │ Context sharing  │ CRUD operations           │
│ Discovery    │ Streaming        │ Well-defined contracts    │
└──────────────┴──────────────────┴───────────────────────────┘
```

## A2A (Agent-to-Agent) Protocol

### What It Is
A2A is a protocol for agents to discover, communicate with, and delegate tasks to other agents.
It enables multi-agent architectures where specialized agents collaborate.

### Key Concepts

**Agent Card** — A JSON document describing an agent's capabilities, published at a well-known URL.
```json
{
  "name": "HR Benefits Agent",
  "url": "https://hr-agent.azurewebsites.net",
  "description": "Handles employee benefits inquiries, PTO balance, enrollment",
  "version": "1.0.0",
  "capabilities": {
    "streaming": true,
    "pushNotifications": false
  },
  "skills": [
    {
      "id": "benefits-lookup",
      "name": "Benefits Lookup",
      "description": "Look up employee benefits details"
    },
    {
      "id": "pto-balance",
      "name": "PTO Balance",
      "description": "Check PTO balance for an employee"
    }
  ],
  "authentication": {
    "schemes": ["oauth2"],
    "credentials": "See agent documentation"
  }
}
```

**Task** — A unit of work sent from one agent to another:
```json
{
  "id": "task-123",
  "sessionId": "session-456",
  "status": "working",
  "message": {
    "role": "user",
    "parts": [
      { "type": "text", "text": "What is the PTO balance for employee #12345?" }
    ]
  }
}
```

**Task Lifecycle:** submitted → working → completed / failed / canceled

### Architecture Patterns

#### Hub-and-Spoke (Router Pattern)
```
User → Router Agent → HR Agent
                    → IT Agent
                    → Finance Agent
```
The router understands all available agents and delegates based on intent classification.

#### Peer-to-Peer
```
Agent A ←→ Agent B ←→ Agent C
```
Agents communicate directly. Good for small, well-defined agent networks.

#### Hierarchical
```
Orchestrator
├── Manager A
│   ├── Worker A1
│   └── Worker A2
└── Manager B
    ├── Worker B1
    └── Worker B2
```
Multi-level delegation for complex workflows.

### Implementation Steps

1. Build each agent as a standalone service with the Agents SDK
2. Publish Agent Cards at `/.well-known/agent.json` on each agent's URL
3. Implement the A2A task handling endpoint
4. Configure authentication between agents (Managed Identity recommended)
5. Build the router/orchestrator that discovers and delegates

## MCP (Model Context Protocol)

### What It Is
MCP standardizes how agents discover and use tools. An MCP server exposes tools that any
MCP-compatible agent can invoke — similar to how a REST API exposes endpoints,
but designed specifically for AI agent tool use.

### Key Concepts

**Server** — Hosts tools and resources. Runs as a service (HTTP/SSE or stdio).
**Client** — The agent that connects to MCP servers and calls tools.
**Tool** — A function the agent can invoke (with name, description, parameters, return type).
**Resource** — Data the agent can read (files, records, configurations).

### Microsoft First-Party MCP Servers

| Server | Capabilities | Connection |
|--------|-------------|------------|
| Outlook | Read/write email, calendar events, contacts | Graph API + MCP wrapper |
| Teams | Send messages, read channels, manage meetings | Graph API + MCP wrapper |
| Search | Microsoft Search across M365 content | Graph API + MCP wrapper |
| Fabric | Query semantic models, access lakehouse data | Fabric API + MCP wrapper |
| D365 | CRM/ERP operations — accounts, contacts, orders | Dataverse API + MCP wrapper |

### Building a Custom MCP Server

Use FastMCP (Python) for rapid MCP server development:

```python
from fastmcp import FastMCP
import os

mcp = FastMCP(
    name="inventory-tools",
    description="Tools for inventory management"
)

@mcp.tool()
async def check_inventory(product_id: str) -> dict:
    """Check current stock levels for a product.

    Args:
        product_id: The product SKU or ID to look up
    """
    result = await inventory_db.query(product_id)
    return {
        "product_id": product_id,
        "stock_level": result.quantity,
        "warehouse": result.location,
        "last_updated": result.updated_at.isoformat()
    }

@mcp.tool()
async def place_order(
    product_id: str,
    quantity: int,
    customer_id: str,
    priority: str = "normal"
) -> dict:
    """Place a purchase order for a customer.

    Args:
        product_id: Product to order
        quantity: Number of units
        customer_id: Customer placing the order
        priority: Order priority (normal, rush, critical)
    """
    order = await orders_api.create(product_id, quantity, customer_id, priority)
    return {"order_id": order.id, "status": "created", "estimated_delivery": order.eta}

@mcp.resource("inventory://products")
async def list_products() -> str:
    """List all available products."""
    products = await inventory_db.list_all()
    return json.dumps(products)

if __name__ == "__main__":
    mcp.run(transport="sse", host="0.0.0.0", port=8080)
```

### Hosting MCP Servers in Azure

**Azure Container Apps (Recommended):**
```bash
# Build and push container
docker build -t myacr.azurecr.io/mcp-inventory:latest .
docker push myacr.azurecr.io/mcp-inventory:latest

# Deploy to Container Apps
az containerapp create \
  --name mcp-inventory \
  --resource-group rg-agents \
  --image myacr.azurecr.io/mcp-inventory:latest \
  --target-port 8080 \
  --ingress external \
  --min-replicas 1 \
  --max-replicas 5
```

**Authentication for MCP Servers:**
- Use Managed Identity for Azure-hosted agents
- Use Entra ID OAuth for cross-tenant scenarios
- Never expose MCP servers publicly without authentication

### Connecting Agent to MCP Server

```python
from mcp import ClientSession
from mcp.client.sse import sse_client

async def connect_to_mcp():
    async with sse_client("https://mcp-inventory.azurecontainerapps.io/sse") as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()
            tools = await session.list_tools()
            result = await session.call_tool("check_inventory", {"product_id": "SKU-123"})
```

## OpenAPI Integration

### What It Is
Standard REST API definitions that agents use to call external services.
Declarative agents use OpenAPI specs as "API plugins" — the agent reads
the spec and knows how to call the API.

### Creating an API Plugin for a Declarative Agent

1. Create or obtain the OpenAPI spec (YAML/JSON)
2. Register it in the agent manifest
3. The agent automatically discovers available operations

```json
// declarativeAgent.json
{
  "actions": [
    {
      "id": "inventoryApi",
      "file": "api-plugins/inventory-openapi.yml"
    }
  ]
}
```

### OpenAPI Spec for Agent Consumption

```yaml
openapi: 3.0.0
info:
  title: Inventory API
  description: API for checking product inventory and placing orders
  version: 1.0.0
servers:
  - url: https://api.contoso.com/v1
paths:
  /products/{productId}/inventory:
    get:
      operationId: checkInventory
      summary: Check inventory levels for a product
      description: Returns current stock level, warehouse location, and last update
      parameters:
        - name: productId
          in: path
          required: true
          schema:
            type: string
          description: The product SKU or ID
      responses:
        '200':
          description: Inventory information
          content:
            application/json:
              schema:
                type: object
                properties:
                  stock_level:
                    type: integer
                  warehouse:
                    type: string
```

### When to Use Each Protocol

| Scenario | Protocol |
|----------|----------|
| Agent calls a REST API | OpenAPI |
| Agent uses AI-aware tools with context | MCP |
| Agent delegates to another agent | A2A |
| Simple CRUD operations | OpenAPI |
| Tools that need conversation context | MCP |
| Multi-agent collaboration | A2A |
| Existing REST APIs | OpenAPI |
| New AI-native tool servers | MCP |
| Autonomous agent networks | A2A |
