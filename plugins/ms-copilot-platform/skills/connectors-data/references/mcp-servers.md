# Microsoft First-Party MCP Servers

Detailed reference for Microsoft's MCP (Model Context Protocol) server implementations
that provide tool access to core Microsoft services.

## Overview

Microsoft provides MCP servers for its major services, allowing agents to interact with
M365 and business data through the standardized MCP protocol. These servers expose tools
that agents can discover and invoke.

## Outlook MCP Server

### Available Tools

| Tool | Description | Permissions Required |
|------|-----------|---------------------|
| `read_email` | Read a specific email by ID | Mail.Read |
| `search_mail` | Search emails by query | Mail.Read |
| `send_email` | Send an email | Mail.Send |
| `create_draft` | Create email draft | Mail.ReadWrite |
| `list_events` | List calendar events | Calendars.Read |
| `create_event` | Create calendar event | Calendars.ReadWrite |
| `get_contacts` | List contacts | Contacts.Read |
| `list_folders` | List mail folders | Mail.Read |

### Authentication
- OAuth 2.0 via Microsoft Entra ID
- Supports delegated (user) and application (service) permissions
- Token caching for performance

### Use Cases
- Email triage and classification agents
- Meeting scheduling assistants
- Contact management agents
- Email response drafting

## Teams MCP Server

### Available Tools

| Tool | Description | Permissions Required |
|------|-----------|---------------------|
| `send_message` | Post message to channel or chat | ChannelMessage.Send |
| `read_messages` | Read channel/chat messages | ChannelMessage.Read.All |
| `list_channels` | List team channels | Channel.ReadBasic.All |
| `list_teams` | List joined teams | Team.ReadBasic.All |
| `get_transcript` | Get meeting transcript | OnlineMeetingTranscript.Read.All |
| `search_teams` | Search across Teams content | TeamsActivity.Read |

### Use Cases
- Team communication agents
- Meeting summary generators
- Channel monitoring agents
- Cross-team information retrieval

## Search MCP Server

### Available Tools

| Tool | Description | Permissions Required |
|------|-----------|---------------------|
| `search_content` | Full-text search across M365 | Sites.Read.All |
| `search_files` | Search OneDrive/SharePoint files | Files.Read.All |
| `search_people` | Search directory for people | People.Read |
| `search_messages` | Search email/Teams messages | Mail.Read |
| `search_events` | Search calendar events | Calendars.Read |

### Use Cases
- Enterprise knowledge retrieval
- People finder agents
- Document discovery
- Cross-service search

## Fabric MCP Server

### Available Tools

| Tool | Description | Permissions Required |
|------|-----------|---------------------|
| `query_semantic_model` | DAX query on semantic model | Dataset.Read.All |
| `execute_sql` | SQL query on warehouse/lakehouse | Workspace.Read.All |
| `list_datasets` | List available datasets | Dataset.Read.All |
| `browse_catalog` | Browse data catalog | Workspace.Read.All |
| `get_table_schema` | Get table structure | Dataset.Read.All |

### Use Cases
- Data analytics agents
- BI query assistants
- Data catalog exploration
- Ad-hoc reporting agents

## D365 MCP Server

### Available Tools

| Tool | Description | Permissions Required |
|------|-----------|---------------------|
| `query_entity` | Query Dataverse entities | Dataverse access |
| `create_record` | Create new record | Dataverse write access |
| `update_record` | Update existing record | Dataverse write access |
| `search_records` | Full-text search records | Dataverse access |
| `get_metadata` | Get entity metadata | Dataverse access |

### Use Cases
- CRM data access agents
- Record management assistants
- Business process automation
- Customer data lookup

## Connection Configuration

### For Custom Engine Agents (Code)

```python
# Python example using MCP client
from mcp import ClientSession
from mcp.client.stdio import stdio_client

async def connect_outlook_mcp():
    server_params = {
        "command": "npx",
        "args": ["-y", "@microsoft/mcp-outlook"],
        "env": {
            "AZURE_TENANT_ID": os.environ["TENANT_ID"],
            "AZURE_CLIENT_ID": os.environ["CLIENT_ID"],
            "AZURE_CLIENT_SECRET": os.environ["CLIENT_SECRET"]
        }
    }
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()
            tools = await session.list_tools()
            # Use tools...
```

### For Copilot Studio
MCP servers are configured as external tool connections in Copilot Studio's
agent settings. Navigate to: Agent → Settings → Tools → Add MCP Server.

### For Declarative Agents
MCP server connections are defined in the agent manifest:
```json
{
  "capabilities": {
    "mcpServers": [
      {
        "id": "outlook",
        "url": "https://mcp.microsoft.com/outlook",
        "authentication": {
          "type": "oauth2",
          "scopes": ["Mail.Read", "Mail.Send"]
        }
      }
    ]
  }
}
```

## Best Practices

1. **Least privilege** — Request only the permissions each MCP server needs
2. **Cache tokens** — Reuse OAuth tokens to reduce auth overhead
3. **Handle rate limits** — Microsoft Graph APIs have rate limits; implement retry logic
4. **Log tool calls** — Record all MCP tool invocations for audit
5. **Test permissions** — Verify Graph API permissions before deploying
6. **Use delegated auth when possible** — Prefer user-delegated over app-only for data access
