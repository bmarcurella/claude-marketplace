---
description: Check your environment readiness for Microsoft agent development — installed tools, MCP servers, extensions, and Azure access.
argument-hint: "[quick|full]"
---

# Environment Status Check

Run a diagnostic of the user's development environment for Microsoft agent development.
Default to **quick** unless the user specifies `full`.

## Quick Check (default)

Detect and report the following. Use shell commands to check for installed tools.

### VS Code Extensions
Check for these extensions using `code --list-extensions`:
- `TeamsDevApp.ms-teams-vscode-extension` (Microsoft Agents Toolkit)
- `ms-azuretools.vscode-azurefunctions` (Azure Functions)
- `ms-azuretools.vscode-docker` (Docker)
- `ms-azuretools.vscode-bicep` (Bicep)
- `ms-python.python` (Python)
- `ms-dotnettools.csharp` (C#)

### CLI Tools
Check version and availability:
- `node --version` and `npm --version`
- `python3 --version` or `python --version`
- `dotnet --version`
- `az version` (Azure CLI)
- `docker --version`
- `git --version`

### MCP Servers
Check the user's MCP configuration for these servers:
- Azure MCP Server
- GitHub MCP Server
- Microsoft Graph MCP
- Microsoft Learn MCP
- Foundry MCP Server

Look in `~/.claude/.mcp.json`, `~/.claude/settings.json`, and the current project's `.mcp.json`.

### Azure CLI Status
- `az account show` — check if logged in and which subscription is active

### Present Results

Format the output as:

```
ENVIRONMENT STATUS
━━━━━━━━━━━━━━━━━

VS Code Extensions:
  ✓ Microsoft Agents Toolkit (v[version])
  ✗ Azure Functions — install: code --install-extension ms-azuretools.vscode-azurefunctions
  ✓ Docker
  ...

CLI Tools:
  ✓ node v22.x / npm v10.x
  ✓ python 3.12.x
  ✗ dotnet — install: https://dot.net/download
  ✓ az 2.x (logged in as user@domain.com, subscription: [name])
  ...

MCP Servers:
  ✓ Microsoft Learn — configured (plugin: ms-copilot-platform)
  ✗ Azure MCP Server — recommended for resource provisioning
  ✗ GitHub MCP Server — recommended for repo and CI/CD automation
  ...

READINESS: [Ready for declarative agents | Ready for custom engine agents | Needs setup]

RECOMMENDED NEXT STEPS:
  1. [Most important missing item and how to install it]
  2. [Second most important]
  ...
```

## Full Check

Everything above, plus:

### Azure Permissions
- `az role assignment list --assignee $(az ad signed-in-user show --query id -o tsv)` — check roles
- Can the user create resource groups? App registrations?

### Entra ID Access
- `az ad app list --filter "displayName eq 'test'" --top 1` — check if app registration is possible

### Agent 365 License
- Note that Agent 365 GA is May 1, 2026. Check if preview access is available.

### Agents Toolkit Templates
- Check if Agents Toolkit CLI is available and list available templates

Present the full report with the same format as quick, plus the additional sections.

## Important

- Do NOT install anything without asking the user first
- For missing items, provide the exact install command or link
- Group recommendations by priority (blocking vs nice-to-have)
- If the user has most tools installed, keep the output brief — don't list 20 green checkmarks
