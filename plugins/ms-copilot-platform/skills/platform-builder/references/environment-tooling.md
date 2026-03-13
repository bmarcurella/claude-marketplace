# Environment Tooling Reference

Maps plan requirements to the VS Code extensions, CLI tools, and MCP servers needed to
build and deploy agents. Used by Phase 0 (Environment Setup) of the platform-builder skill.

## VS Code Extensions

| Extension | When Required | What It Does |
|-----------|--------------|-------------|
| **Microsoft Agents Toolkit** | Always (pro-code agents) | Scaffolds, tests, provisions, deploys agents |
| **Python** (ms-python.python) | Plan language = Python | IntelliSense, debugging, linting |
| **C#** (ms-dotnettools.csharp) | Plan language = C# | IntelliSense, debugging, .NET support |
| **Docker** (ms-azuretools.vscode-docker) | Plan uses containers | Dockerfile editing, container management |
| **Bicep** (ms-azuretools.vscode-bicep) | Plan uses IaC | Bicep authoring, validation, deployment |
| **Azure Functions** (ms-azuretools.vscode-azurefunctions) | Plan uses Functions hosting | Function development, local debugging |
| **Azure Resources** (ms-azuretools.vscode-azureresourcegroups) | Plan deploys to Azure | Browse and manage Azure resources |
| **GitHub Copilot** | Recommended always | AI-assisted coding with project CLAUDE.md context |

## CLI Tools

| Tool | When Required | Install Command | Verify Command |
|------|--------------|-----------------|----------------|
| **Node.js 20+** | TypeScript agents, Agents Toolkit | Download from nodejs.org | `node --version` |
| **Python 3.11+** | Python agents | Download from python.org | `python --version` |
| **.NET 8+** | C# agents | `dotnet-install` script or download | `dotnet --version` |
| **Azure CLI** | Any Azure deployment | `brew install azure-cli` (macOS) | `az --version` |
| **Docker Desktop** | Container-based agents, MCP servers | Download from docker.com | `docker --version` |
| **git** | Always | Pre-installed on most systems | `git --version` |
| **Agents Toolkit CLI** | Always (pro-code) | Installed with VS Code extension, or `npm install -g @microsoft/agents-toolkit-cli` | `agents-toolkit --version` |

## MCP Servers — Automation Helpers

These MCP servers let the AI assistant automate parts of the build that the user may not
be familiar with. They're optional but significantly reduce friction.

### Azure MCP Server
- **What it automates:** Resource group creation, App Service / Container Apps provisioning,
  Key Vault setup, Azure OpenAI deployment, Function App creation, monitoring configuration
- **When to recommend:** User isn't comfortable with Azure portal or Azure CLI
- **Install:** Follow Azure MCP Server setup docs
- **Configuration:** Needs Azure subscription ID and authentication (az login or managed identity)

### Microsoft Graph MCP Server
- **What it automates:** Entra app registrations, API permission grants, service principal
  creation, conditional access policy review
- **When to recommend:** User isn't comfortable with Entra ID / Azure AD administration
- **Configuration:** Needs Graph API permissions (Application.ReadWrite.All for app registration)

### GitHub MCP Server
- **What it automates:** Repository creation, branch protection rules, GitHub Actions
  workflow files, secret management, code push
- **When to recommend:** User wants CI/CD automation or repo setup handled automatically
- **Configuration:** Needs GitHub personal access token with repo and workflow scopes

### Azure AI Foundry MCP Server
- **What it automates:** Model deployment, endpoint configuration, evaluation pipelines,
  responsible AI configuration
- **When to recommend:** Plan uses Foundry for model hosting and user isn't familiar with it
- **Configuration:** Needs Foundry workspace access and Azure authentication

### Azure DevOps MCP Server
- **What it automates:** Pipeline creation, release management, work item tracking,
  artifact publishing
- **When to recommend:** Organization uses Azure DevOps instead of GitHub
- **Configuration:** Needs Azure DevOps PAT with appropriate scopes

## Mapping Plan Requirements to Tooling

Use this table to determine what to check for based on the plan:

| Plan Element | Required Extensions | Required CLIs | Recommended MCP Servers |
|-------------|-------------------|---------------|------------------------|
| **Declarative agent** | Agents Toolkit | Node.js, git | GitHub MCP |
| **Custom engine (Python)** | Agents Toolkit, Python | Python, git, Docker | Azure MCP, GitHub MCP |
| **Custom engine (C#)** | Agents Toolkit, C# | .NET, git, Docker | Azure MCP, GitHub MCP |
| **Custom engine (TypeScript)** | Agents Toolkit | Node.js, git, Docker | Azure MCP, GitHub MCP |
| **Azure hosting** | Azure Resources | Azure CLI | Azure MCP |
| **Azure Functions hosting** | Azure Functions | Azure CLI, func CLI | Azure MCP |
| **Foundry model hosting** | — | Azure CLI | Foundry MCP, Azure MCP |
| **Entra app registration** | — | Azure CLI (az ad) | Microsoft Graph MCP |
| **GitHub CI/CD** | — | git, gh CLI | GitHub MCP |
| **Azure DevOps CI/CD** | — | az devops | Azure DevOps MCP |
| **Infrastructure as Code** | Bicep | Azure CLI | Azure MCP |
| **Container deployment** | Docker | Docker CLI | Azure MCP |

## Detection Commands

Use these commands to check what's already installed:

```bash
# VS Code extensions
code --list-extensions | grep -i "agents-toolkit\|python\|csharp\|docker\|bicep\|azure"

# CLI tools
node --version 2>/dev/null && echo "Node.js: installed" || echo "Node.js: missing"
python3 --version 2>/dev/null && echo "Python: installed" || echo "Python: missing"
dotnet --version 2>/dev/null && echo ".NET: installed" || echo ".NET: missing"
az --version 2>/dev/null | head -1 && echo "Azure CLI: installed" || echo "Azure CLI: missing"
docker --version 2>/dev/null && echo "Docker: installed" || echo "Docker: missing"
git --version 2>/dev/null && echo "git: installed" || echo "git: missing"
```

## Licensing Requirements

| Component | License Needed |
|-----------|---------------|
| Declarative agents (basic) | Microsoft 365 (some features without Copilot license) |
| Declarative agents (full) | Microsoft 365 Copilot license |
| Copilot Studio | Copilot Studio license (standalone or included in some M365 plans) |
| Custom engine in Teams only | No Copilot license required |
| Custom engine in M365 Copilot | M365 Copilot license + metering enabled |
| Agent 365 governance | Agent 365 license ($15/user/month or qualifying M365 plan) |
| Azure hosting | Azure subscription (pay-as-you-go) |
| Azure OpenAI | Azure subscription + OpenAI resource quota |

Use `microsoft_docs_search` to verify current licensing — these change frequently.
