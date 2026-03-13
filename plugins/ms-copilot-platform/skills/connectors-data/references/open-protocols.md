# Open Protocols — OpenAPI, MCP, and A2A

Detailed reference for the three open protocols that enable interoperability across the
Microsoft Frontier Agent Platform.

## OpenAPI (Open API Specification)

### Overview
OpenAPI (formerly Swagger) is the standard for describing REST APIs. In the agent platform,
OpenAPI specs define how agents call external APIs.

### Usage in Agents
- **Declarative agents**: API plugins are defined via OpenAPI specs in the agent manifest
- **Copilot Studio**: Custom connectors use OpenAPI specs to define available operations
- **Custom engine agents**: Can call any API with an OpenAPI spec

### Key Elements for Agent Integration
```yaml
openapi: 3.0.0
info:
  title: My Agent API Plugin
  version: 1.0.0
paths:
  /search:
    get:
      operationId: searchRecords
      summary: Search for records by query
      description: >
        This description helps the AI understand WHEN to call this API.
        Write it for the LLM, not just for developers.
      parameters:
        - name: query
          in: query
          required: true
          schema:
            type: string
          description: The search query from the user
```

**Best practice:** Write operation descriptions that help the LLM understand when and how
to use each endpoint. The `description` field is effectively a prompt for the AI.

## MCP (Model Context Protocol)

### Overview
MCP is a standardized protocol for agent-tool communication. It allows agents to discover
and invoke tools exposed by MCP servers, with structured input/output schemas.

### Architecture
```
Agent (MCP Client) ←→ MCP Server ←→ Backend Service
                   JSON-RPC over
                   stdio/SSE/HTTP
```

### Microsoft's MCP Servers
Microsoft provides first-party MCP servers for core services:

#### Outlook MCP Server
Tools: read_email, send_email, search_mail, create_event, list_events, get_contacts
Auth: OAuth 2.0 via Entra ID (delegated or app-only)

#### Teams MCP Server
Tools: send_message, read_messages, list_channels, get_meeting_transcript, search_teams
Auth: OAuth 2.0 via Entra ID

#### Search MCP Server
Tools: search_content, search_people, search_files, search_messages
Auth: OAuth 2.0 via Entra ID (uses Microsoft Search API)

#### Fabric MCP Server
Tools: query_semantic_model, list_datasets, execute_sql, browse_catalog
Auth: OAuth 2.0 / Service Principal

#### D365 MCP Server
Tools: query_entity, create_record, update_record, search_records
Auth: OAuth 2.0 / Service Principal with Dataverse permissions

### MCP Connection Setup
```json
{
  "mcpServers": {
    "outlook": {
      "command": "npx",
      "args": ["-y", "@microsoft/mcp-outlook"],
      "env": {
        "AZURE_TENANT_ID": "${TENANT_ID}",
        "AZURE_CLIENT_ID": "${CLIENT_ID}"
      }
    },
    "teams": {
      "command": "npx",
      "args": ["-y", "@microsoft/mcp-teams"],
      "env": {
        "AZURE_TENANT_ID": "${TENANT_ID}",
        "AZURE_CLIENT_ID": "${CLIENT_ID}"
      }
    }
  }
}
```

### Building Custom MCP Servers
For custom integrations, build MCP servers using:
- **Python**: FastMCP framework
- **Node.js/TypeScript**: MCP SDK
- **C#/.NET**: MCP SDK for .NET

## A2A (Agent-to-Agent Protocol)

### Overview
A2A enables multi-agent communication and orchestration. Agents discover each other's
capabilities and delegate tasks through a standardized protocol.

### Key Concepts

#### Agent Card
Every A2A-capable agent publishes an "agent card" describing its capabilities:
```json
{
  "name": "HR Benefits Agent",
  "description": "Answers questions about employee benefits and enrollment",
  "capabilities": [
    {
      "name": "benefits-inquiry",
      "description": "Answer questions about health, dental, vision, and 401k benefits",
      "inputSchema": {
        "type": "object",
        "properties": {
          "question": { "type": "string" }
        }
      }
    }
  ],
  "authentication": {
    "type": "oauth2",
    "tokenUrl": "https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token"
  }
}
```

#### Task Delegation
A coordinator agent sends tasks to specialized agents:
1. Coordinator receives user request
2. Coordinator examines agent cards to find capable agents
3. Coordinator creates a task with input data
4. Specialized agent processes the task
5. Specialized agent returns results
6. Coordinator aggregates and presents to user

#### Multi-Agent Patterns
- **Fan-out**: One request sent to multiple agents in parallel
- **Sequential**: Chain of agents, each processing output of the previous
- **Router**: Coordinator routes to the single most appropriate agent
- **Collaborative**: Multiple agents work together, sharing context

### When to Use A2A
- Complex requests spanning multiple domains
- Specialized agents owned by different teams
- Load distribution across agent services
- Composing simple agents into complex workflows

## Protocol Comparison

| Feature | OpenAPI | MCP | A2A |
|---------|---------|-----|-----|
| Purpose | API integration | Tool access | Agent coordination |
| Communication | HTTP REST | JSON-RPC | JSON-RPC / HTTP |
| Discovery | API spec document | Tool listing | Agent cards |
| Auth | API keys, OAuth | OAuth, tokens | OAuth, mTLS |
| Statefulness | Stateless | Can be stateful | Task-based state |
| Best for | Known APIs | Dynamic tools | Multi-agent systems |

## Integration Decision Guide

- **Calling a well-known REST API?** → OpenAPI
- **Agent needs to dynamically discover and use tools?** → MCP
- **Agents need to talk to each other?** → A2A
- **All three?** → Common in enterprise solutions
