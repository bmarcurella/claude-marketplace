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

# Platform Builder — Automated Solution Deployment

The automation engine for the Microsoft Frontier Agent Platform. Turns approved architecture
designs into deployable artifacts. Operates in two modes: guided walkthrough for learning,
or autonomous build for speed.

## Core Principle: Build with Understanding

Every action — whether guided or autonomous — includes:
1. **WHAT** is being created
2. **WHY** it's needed (linked to the architecture design)
3. **WHAT'S REQUIRED** before it can proceed (prerequisites, permissions)
4. **WHAT IT COSTS** (licensing and resource implications)
5. **WHAT TO WATCH FOR** (risks, gotchas, common mistakes)

## Two Execution Modes

### Mode A: Guided Walkthrough (Default)

Walk through each phase step-by-step. At every step:

```
Step [N.M]: [Action Name]
─────────────────────────
📋 WHAT: [What is being created]
🎯 WHY:  [Why it matters architecturally]
🔧 HOW:  [Exact commands, files, configurations]
✅ VERIFY: [How to confirm success]
💡 LEARN: [Architecture concept + docs link]
⚠️ TROUBLESHOOT: [Common failures and fixes]
```

Present one step at a time. Wait for confirmation. Explain concepts as they arise.
This mode is ideal for learning, team enablement, and first-time builds.

### Mode B: Autonomous Build (Claude Code in VS Code)

Generate all artifacts at once — a complete, deployable project. Produces:

```
[project-name]/
├── README.md              # Full architecture explanation + setup guide
├── src/                   # All application code
├── infra/                 # Bicep templates for Azure provisioning
├── Dockerfile             # Container definition
├── docker-compose.yml     # Local development environment
├── scripts/
│   ├── deploy.sh          # One-command deployment
│   ├── cleanup.sh         # Complete teardown
│   └── setup-local.sh     # Local environment bootstrapping
├── tests/                 # Test suite
├── .env.example           # Environment variable template
└── .github/workflows/     # CI/CD pipeline
```

The README serves as the architecture document — it explains every decision,
every component, and every configuration choice.

### Mode C: Hybrid (Recommended)

Autonomous for infrastructure and boilerplate. Guided for agent logic and customization.

1. **Auto-generate:** Project scaffold, Bicep, Docker, deployment scripts
2. **Guide through:** AI configuration, custom instructions, tool implementation
3. **Auto-generate:** Tests, CI/CD, monitoring setup
4. **Guide through:** First deployment, testing, Agent 365 registration

## Build Process

### Phase 1: Design Validation

Before building, validate the architecture design:

1. **Completeness check** — All required components specified?
2. **Compatibility check** — Components work together?
3. **Prerequisites check** — Licenses, permissions, environments available?
4. **Cost estimation** — Azure resources, licenses, connectors
5. **Security review** — Follows best practices?

Present a **Build Readiness Report:**

```
BUILD READINESS REPORT
━━━━━━━━━━━━━━━━━━━━━
🟢 READY
   ✓ Azure subscription active
   ✓ Azure OpenAI quota available
   ✓ VS Code + Agents Toolkit installed

🟡 ATTENTION NEEDED (can proceed)
   ⚡ Agent 365 license not detected — governance optional for dev
   ⚡ Custom domain not configured — can use default Azure URL

🔴 MUST RESOLVE
   ✗ No Entra admin consent — cannot register app
   ✗ Missing Key Vault access — need Key Vault Secrets Officer role
```

Read `references/validation-rules.md` for the complete ruleset.

### Phase 2: Build Plan Generation

Generate a sequenced plan matching the architecture design:

```
Build Plan: [Solution Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━

Phase 1: Foundation (Azure Resources)     [~30 min]
  ├── 1.1 Resource Group
  ├── 1.2 Managed Identity
  ├── 1.3 Key Vault
  └── 1.4 Application Insights

Phase 2: AI Services                      [~20 min]
  ├── 2.1 Azure OpenAI deployment
  └── 2.2 Model configuration

Phase 3: Agent Infrastructure             [~1 hour]
  ├── 3.1 Project scaffold (from template)
  ├── 3.2 Agent code (handler + tools)
  ├── 3.3 Docker configuration
  └── 3.4 Local testing

Phase 4: Integrations                     [~1 hour]
  ├── 4.1 MCP server setup (if applicable)
  ├── 4.2 Connector configurations
  ├── 4.3 Knowledge source setup
  └── 4.4 Integration testing

Phase 5: Governance & Deployment          [~45 min]
  ├── 5.1 Entra app registration
  ├── 5.2 Bot Service registration
  ├── 5.3 Agent 365 registration
  ├── 5.4 Deploy to Azure
  └── 5.5 Publish to channels
```

### Phase 3: Artifact Generation

Use project templates from `agent-development/references/project-templates.md`.
Generate artifacts for each category:

**Infrastructure as Code:**
- Bicep templates — modular, parameterized, with cleanup scripts
- Docker files — agent containers, MCP server containers
- CI/CD pipelines — GitHub Actions or Azure DevOps

Read `references/iac-templates.md` for Bicep patterns.

**Agent Code:**
- Project scaffold from the appropriate template (T1-T8)
- Domain-specific tools
- Custom instructions / system prompts
- Test files

Read `references/agent-scaffolds.md` for scaffolding details.

**Connector & Integration:**
- MCP server code (if hosting custom tools)
- OpenAPI specs for API plugins
- Power Automate flow definitions
- Graph API permissions manifest

**Governance:**
- Entra app registration manifest
- RBAC role assignments
- Agent 365 registration config
- Monitoring dashboard definitions

### Phase 4: Execution

**In Guided mode:** Present each step with the WHAT/WHY/HOW/VERIFY/LEARN format.

**In Autonomous mode:** Generate all files, then present a summary:
```
✅ PROJECT GENERATED: contoso-support-agent/
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Files created: 24
Architecture: Custom Engine (Python + Semantic Kernel)
Infrastructure: 5 Bicep modules (App Service, Bot, KV, Identity, Monitoring)
Tools: 3 (customer_lookup, create_ticket, check_status)
Tests: 8 test cases

NEXT STEPS:
1. Copy .env.example to .env and fill in values
2. Run: ./scripts/setup-local.sh
3. Run: docker-compose up
4. Test in Agents Playground at http://localhost:5000
5. Deploy: ./scripts/deploy.sh

Full documentation in README.md
```

### Phase 5: Validation

After building, verify the solution works:

1. **Smoke test** — Agent responds to basic messages
2. **Tool test** — Each tool function executes correctly
3. **Integration test** — Cross-component communication works
4. **Security test** — Permissions are least-privilege
5. **Governance test** — Agent 365 registration confirmed

## Safety and Guardrails

**Non-negotiable (always enforced):**
- Never store secrets in plain text — Key Vault only
- Never assign Owner/Global Admin automatically
- Always create in user's specified subscription/region
- Always include cleanup/teardown scripts
- Always estimate cost before provisioning
- Require explicit approval for production deployments

**Configurable:**
- Auto-approval for dev/sandbox environments
- Cost thresholds for additional approval
- Custom naming conventions and tagging
- Required reviewers for specific resource types

Read `references/automation-guardrails.md` for the complete framework.

## MCP Server Integration for Automation

When available, use MCP servers for automated execution:

| MCP Server | Automation Capability |
|-----------|---------------------|
| **Azure CLI** | Resource provisioning, configuration |
| **Microsoft Graph** | App registrations, permissions |
| **Power Platform CLI** | Copilot Studio import/export |
| **GitHub** | Repository creation, code push |
| **Azure DevOps** | Pipeline creation, deployment |

## Reference Files

- **`references/validation-rules.md`** — Design validation ruleset
- **`references/iac-templates.md`** — Bicep/ARM/Terraform patterns
- **`references/agent-scaffolds.md`** — Agent project scaffolds
- **`references/automation-guardrails.md`** — Safety framework
- **`references/mcp-automation.md`** — MCP server automation patterns
