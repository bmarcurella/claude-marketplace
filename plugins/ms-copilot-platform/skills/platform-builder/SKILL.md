---
name: platform-builder
description: >
  This skill should be used when the user asks to "build the infrastructure", "automate the
  deployment", "create everything from the design", "provision the agent environment",
  "generate the Bicep templates", "auto-build after approval", "scaffold the entire solution",
  "generate ARM templates", "set up the Azure resources", "create the Power Platform environment",
  "deploy the agent stack", "infrastructure as code for agents", "automate agent setup",
  "build from the architecture plan", "execute the design", "build it for me",
  "walk me through building it", "set up Docker for my agent", "create the MCP server",
  "scaffold the project", "generate the deployment scripts", or any request to automatically
  generate and provision the infrastructure, code, and configurations needed to implement a
  designed agent solution. This is the automation engine that turns approved architecture
  designs into deployable artifacts with two modes: guided walkthrough (learn while building)
  and autonomous build (Claude Code generates everything).
---

# Platform Builder — Build, Scaffold, and Deploy

The execution engine for the Microsoft Frontier Agent Platform. Takes an approved plan from
the `agent-development` skill and turns it into a working project. Uses the **Microsoft
Agents Toolkit** for scaffolding and adds infrastructure, configuration, and governance
on top.

## Core Principles

1. **Agents Toolkit first** — Scaffold via `agents-toolkit new`, not custom file generation.
   The Toolkit templates are maintained by Microsoft and stay current with SDK versions.
2. **Environment-aware** — Detect what the user has installed and help fill gaps, including
   MCP servers that can automate unfamiliar technologies.
3. **CLAUDE.md on every project** — Generate a project-specific CLAUDE.md so any AI
   assistant (Claude Code, GitHub Copilot) has full context in future sessions.
4. **Teach as you go** — Every action explains what and why, not just how.

---

## Phase 0: Environment Setup

Before building anything, check what the user has and what they need. This phase is
critical — it prevents the "I followed the steps but nothing works" experience.

### Detect Installed Tooling

Check for the tools the plan requires:

```
ENVIRONMENT CHECK
━━━━━━━━━━━━━━━━

VS Code Extensions:
  ✓ / ✗  Microsoft Agents Toolkit
  ✓ / ✗  Azure Functions (if plan uses Functions)
  ✓ / ✗  Docker (if plan uses containers)
  ✓ / ✗  Bicep (if plan uses IaC)
  ✓ / ✗  Python / C# / Node.js extension (matching plan language)

CLI Tools:
  ✓ / ✗  node / npm (version)
  ✓ / ✗  python (version)
  ✓ / ✗  dotnet (version)
  ✓ / ✗  Azure CLI (az)
  ✓ / ✗  Docker Desktop
  ✓ / ✗  git

MCP Servers (configured in user's environment):
  ✓ / ✗  Azure MCP Server — automates resource provisioning
  ✓ / ✗  GitHub MCP Server — automates repo setup and CI/CD
  ✓ / ✗  Foundry MCP Server — automates model deployment (if plan uses Foundry)
  ✓ / ✗  Microsoft Graph MCP — automates Entra app registration
```

### Recommend MCP Servers for Unfamiliar Technologies

This is the key value-add. If the plan requires a technology the user isn't comfortable
with, recommend an MCP server that lets the AI assistant handle it:

| Plan Requires | User Isn't Comfortable With | Recommend |
|---------------|----------------------------|-----------|
| Azure Container Apps | Container deployment | **Azure MCP Server** — provisions and deploys containers |
| Azure Functions | Serverless setup | **Azure MCP Server** — creates and configures Function Apps |
| Azure AI Foundry | Model management | **Foundry MCP Server** — deploys models, configures endpoints |
| Entra app registration | Identity/auth setup | **Microsoft Graph MCP** — registers apps, sets permissions |
| GitHub Actions CI/CD | Pipeline configuration | **GitHub MCP Server** — creates workflows, manages repos |
| Azure OpenAI | Model deployment | **Azure MCP Server** — provisions OpenAI resources |

Read `references/environment-tooling.md` for the full mapping of plan requirements to
MCP servers, extensions, and CLIs.

### Install and Configure

For each missing requirement:
1. Explain what it does and why the plan needs it
2. Offer to install/configure it (with user permission)
3. For MCP servers: add to the project's `.mcp.json` or the user's MCP configuration
4. Verify the installation works

---

## Phase 1: Design Validation

Validate the plan from `agent-development` before building:

1. **Completeness** — All required components specified?
2. **Compatibility** — Components work together?
3. **Prerequisites** — Licenses, permissions, environments available?
4. **Cost estimation** — Azure resources, licenses, connectors
5. **Security** — Follows least-privilege and Key Vault patterns?

Present a **Build Readiness Report:**

```
BUILD READINESS REPORT
━━━━━━━━━━━━━━━━━━━━━

READY:
  ✓ Azure subscription active
  ✓ Agents Toolkit installed (v[version])
  ✓ Python 3.11+ available
  ✓ Azure MCP Server configured — will handle resource provisioning

ATTENTION NEEDED (can proceed):
  ~ Agent 365 license not detected — governance optional for dev
  ~ Custom domain not configured — can use default Azure URL

MUST RESOLVE:
  ✗ No Entra admin consent — cannot register app
  ✗ Missing Key Vault access — need Key Vault Secrets Officer role
```

Read `references/validation-rules.md` for the complete ruleset.

---

## Phase 2: Scaffold via Agents Toolkit

Run the appropriate Agents Toolkit template command from the plan:

```bash
# Example: Custom engine agent in Python
agents-toolkit new custom-engine-agent --lang python --name "contoso-support"

# Example: Declarative agent
agents-toolkit new declarative-agent --name "hr-helper"
```

The Toolkit generates the base project structure. Do NOT generate these files manually —
the Toolkit templates stay current with SDK versions and include correct dependencies.

After scaffolding, explain the project structure to the user:
- What each directory and key file does
- Where they'll add their custom logic
- What the Toolkit manages vs what we'll configure

---

## Phase 3: Generate Project CLAUDE.md

Immediately after scaffolding, generate a `CLAUDE.md` at the project root. This file
captures all decisions from the plan so any AI assistant understands the project.

Read `references/project-claude-template.md` for the template. The CLAUDE.md includes:

- **Project identity** — what this agent is, who it's for, where it runs
- **Architecture decisions** — agent type, language, framework, and why each was chosen
- **Key files** — what each important file does and where custom logic lives
- **Connectors and data** — what systems the agent connects to and how
- **Build/test/deploy commands** — the exact commands for each lifecycle stage
- **MCP servers configured** — what's available for automation in this project
- **Prerequisites verified** — what was confirmed during environment setup
- **Conventions** — naming, error handling, testing patterns for this project

This CLAUDE.md is a living document — it should be updated as the project evolves.

---

## Phase 4: Configure the Agent

With the scaffold in place, layer on the plan's custom configuration:

### Agent Instructions
- Write or refine the system prompt / `instructions.md`
- Define role, boundaries, tone, fallback behavior
- Add negative instructions (what the agent must NOT do)

### Knowledge Sources
- Configure SharePoint sites, uploaded files, or web sources
- Set up Copilot connectors for external data

### Actions and Tools
- Implement custom tools/functions defined in the plan
- Configure OpenAPI plugins for external APIs
- Set up MCP server connections

### Manifest and Configuration
- Update the app manifest with correct metadata
- Configure authentication flows (Entra ID)
- Set up environment variables (`.env` from `.env.example`)

---

## Phase 5: Infrastructure (Beyond the Agent)

For custom engine agents, set up the hosting and supporting infrastructure:

### Infrastructure as Code
- Generate Bicep templates for Azure resources (modular, parameterized)
- Include cleanup/teardown scripts
- If Azure MCP Server is configured, use it to provision resources directly

Read `references/iac-templates.md` for Bicep patterns.

### Docker Configuration
- Dockerfile for the agent container
- docker-compose.yml for local development (agent + any MCP servers)
- Container registry configuration for deployment

### CI/CD Pipeline
- GitHub Actions or Azure DevOps pipeline
- Build → Test → Deploy stages
- Environment-specific configurations (dev, staging, prod)

---

## Phase 6: Test and Validate

### Local Testing
- Run via Agents Toolkit test harness (Agents Playground)
- Test each tool/action individually
- Test conversation flows end-to-end
- Verify connector integrations

### Integration Testing
- Sideload to Teams / M365 for real-environment testing
- Test with actual data sources
- Verify authentication flows

### Validation Checklist
1. Agent responds to basic messages
2. Each tool function executes correctly
3. Connectors return expected data
4. Authentication works for all flows
5. Error handling covers failure cases

---

## Phase 7: Deploy and Govern

### Deploy
- Provision Azure resources (via Agents Toolkit or Azure MCP Server)
- Deploy agent code
- Configure DNS and endpoints
- Verify the deployment

### Governance
- Register with Agent 365 (if enterprise deployment)
- Configure Entra ID permissions
- Set up monitoring (Application Insights, Copilot Analytics)
- Publish to org catalog or Teams store

---

## Execution Modes

### Guided (Default)
Walk through each phase step-by-step. At every step:

```
WHAT:        [What is being created or configured]
WHY:         [Why it matters — linked to the plan]
HOW:         [Exact command or file change]
VERIFY:      [How to confirm it worked]
LEARN:       [What concept this teaches]
TROUBLESHOOT: [Common failures and fixes]
```

Present one step at a time. Wait for confirmation. Explain as you go.

### Autonomous
Generate and execute everything, then present a summary:

```
PROJECT BUILT: [project-name]/
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Scaffolded: agents-toolkit new [template] --lang [language]
Files added: [count] (infra, tools, config, tests, CLAUDE.md)
MCP servers: [configured servers]

NEXT STEPS:
1. Review CLAUDE.md for full project context
2. Copy .env.example to .env and fill in values
3. Run: [local dev command]
4. Test in Agents Playground
5. Deploy: [deploy command]
```

### Hybrid (Recommended)
Autonomous for scaffolding, infra, and boilerplate. Guided for agent instructions,
tools, and connectors where the user's domain knowledge matters.

Read `references/execution-modes.md` for detailed guidance.

---

## Safety and Guardrails

**Non-negotiable (always enforced):**
- Never store secrets in plain text — Key Vault only
- Never assign Owner/Global Admin automatically
- Always use the user's specified subscription/region
- Always include cleanup/teardown scripts
- Always estimate cost before provisioning production resources
- Require explicit approval for production deployments
- Always generate CLAUDE.md with every project

**Configurable:**
- Auto-approval for dev/sandbox environments
- Cost thresholds for additional approval
- Custom naming conventions and tagging
- Required reviewers for specific resource types

Read `references/automation-guardrails.md` for the complete framework.

---

## MCP Server Integration for Automation

When MCP servers are available, use them instead of asking the user to run CLI commands
they may not understand:

| MCP Server | What It Automates |
|-----------|-------------------|
| **Azure MCP Server** | Resource groups, App Service, Container Apps, Key Vault, OpenAI, Functions |
| **Microsoft Graph MCP** | Entra app registrations, permissions, service principals |
| **GitHub MCP Server** | Repository creation, branch protection, Actions workflows, code push |
| **Azure DevOps MCP** | Pipeline creation, deployment, work items |
| **Power Platform CLI** | Copilot Studio solution import/export, environment management |
| **Foundry MCP Server** | Model deployment, endpoint configuration, evaluations |

Read `references/mcp-automation.md` for automation patterns with each server.

---

## Reference Files

- **`references/environment-tooling.md`** — MCP server, extension, and CLI mapping for plan requirements
- **`references/project-claude-template.md`** — Template for generating project CLAUDE.md files
- **`references/validation-rules.md`** — Design validation ruleset
- **`references/iac-templates.md`** — Bicep/ARM patterns for agent infrastructure
- **`references/agent-scaffolds.md`** — Agents Toolkit scaffold commands and customization
- **`references/automation-guardrails.md`** — Safety framework for automated builds
- **`references/mcp-automation.md`** — MCP server automation patterns
