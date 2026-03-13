# MCP Server Integration for Automation

Patterns for using MCP servers to automate the build and deployment process.

## Available Automation MCP Servers

### Azure CLI MCP
Automates Azure resource provisioning and management.
```json
{
  "mcpServers": {
    "azure-cli": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-azure-cli"]
    }
  }
}
```
**Capabilities:** Resource group management, Bicep deployment, App Service
configuration, Key Vault operations, Bot Service setup.

### Microsoft Graph MCP
Automates Entra ID and M365 configuration.
**Capabilities:** App registrations, permission grants, user assignments,
Teams app catalog management.

### Power Platform CLI MCP
Automates Copilot Studio and Power Platform operations.
**Capabilities:** Solution export/import, environment management,
connector configuration, flow deployment.

### GitHub MCP
Automates code repository and CI/CD setup.
**Capabilities:** Repository creation, branch management, workflow
creation, secret management.

## Automation Workflow

### Step 1: Check Available MCP Servers
Before automating, verify which MCP servers are connected:
```
List available tools → Check for azure, graph, github capabilities
```

### Step 2: Plan Automation Sequence
Map build plan steps to MCP server capabilities:
```
Phase 1 (Azure Resources) → Azure CLI MCP
Phase 2 (App Registration) → Microsoft Graph MCP
Phase 3 (Code Repository) → GitHub MCP
Phase 4 (Deployment) → Azure CLI MCP
Phase 5 (Teams Publishing) → Microsoft Graph MCP
```

### Step 3: Execute with Guardrails
- Verify each step before proceeding to the next
- Log all actions for audit trail
- Pause before destructive or costly operations
- Provide rollback commands for every step

## Fallback: Manual Commands
When MCP servers are not available, output the equivalent CLI commands
for the user to execute manually. Always provide both options.
