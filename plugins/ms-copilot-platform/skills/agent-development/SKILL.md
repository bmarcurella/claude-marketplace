---
name: agent-development
description: >
  This skill should be used when the user asks to "build an agent", "create a Copilot agent",
  "develop a Microsoft agent", "set up Copilot Studio", "create a declarative agent",
  "build a custom engine agent", "use the M365 Agents SDK", "build with Azure AI Foundry",
  "create a Power Platform agent", "make a Teams bot", "develop an agent for Microsoft 365",
  "what agent type should I use", "build an autonomous agent", "use A2A protocol",
  "use LangChain with Microsoft agents", "use Semantic Kernel", "host an MCP server",
  "build a multi-agent system", "set up agent infrastructure", or any variation of building,
  developing, or creating AI agents on the Microsoft platform. Covers the full spectrum from
  zero-code (SharePoint Agent, Agent Builder) through low-code (Copilot Studio) to pro-code
  (Agents SDK, Foundry, Agent Framework). Also triggers for agent review, improvement,
  troubleshooting, and making agents more capable or agentic.
---

# Agent Development on the Microsoft Frontier Platform

This skill guides users through building AI agents on the Microsoft platform. It uses the
**Microsoft Agents Toolkit** as the default scaffolding and development tool for all pro-code
agents. The workflow is conversational — not an interrogation.

## Core Principle: Listen, Infer, Recommend

Do NOT ask a long list of discovery questions. Instead:

1. **Ask the user to describe what their agent should do** — one open-ended prompt
2. **Infer** the agent type, framework, language, connectors, and hosting from their description
3. **Present a recommendation** with your reasoning — let them adjust
4. **Build the plan** from the confirmed recommendation

If you need clarification, ask ONE focused follow-up — not a checklist.

---

## Step 1: Understand — One Question, Then Infer

Ask the user:

> **"Describe what you want your agent to do, who will use it, and where it should live."**

From their answer, infer these decisions:

| Decision | How to Infer |
|----------|-------------|
| **Agent type** | Does it need its own AI model or custom orchestration? → Custom Engine. Otherwise → Declarative. Does it need no code at all? → Redirect to Agent Builder or Copilot Studio. |
| **Language** | What does the user already work in? Default to Python for data/AI users, C# for .NET/enterprise, TypeScript for web/JS developers. If unclear, recommend Python (widest AI ecosystem). |
| **Framework** | Semantic Kernel for Microsoft-integrated agents. LangChain for complex reasoning chains. If simple, no framework needed — just the Agents SDK directly. |
| **Connectors** | What systems did they mention? Map to MCP servers, OpenAPI plugins, Graph API, or Power Platform connectors. |
| **Hosting** | Azure Container Apps for most custom engine agents. Azure App Service for simpler deployments. No hosting needed for declarative agents. |
| **Channels** | Did they mention Teams, Copilot, web, email? Default to M365 Copilot + Teams unless stated otherwise. |

### When to Redirect Away from Agents Toolkit

Not everything needs pro-code. Redirect when appropriate:

| Signal in User's Description | Redirect To |
|------------------------------|-------------|
| "Just answer questions from our SharePoint site" | **SharePoint Agent** (zero code, 2 minutes) |
| "Simple Q&A with custom instructions" | **Agent Builder** in Copilot Chat |
| "Connect to Salesforce/SAP/ServiceNow with workflows" | **Copilot Studio** (visual builder, 1000+ connectors) |
| "No code" / "I've never coded" | **Copilot Studio** or **Agent Builder** |

For these paths, read `references/zero-code-agents.md` or `references/copilot-studio-guide.md`
and guide accordingly. The rest of this workflow assumes the Agents Toolkit path.

---

## Step 2: Present the Recommendation

Present your best recommendation with reasoning. One recommendation, not three — the user
can ask for alternatives if they want them.

**Format:**

```
RECOMMENDATION: [Agent Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Type:       [Declarative Agent | Custom Engine Agent]
Language:   [Python | C# | TypeScript]
Framework:  [Semantic Kernel | LangChain | Agents SDK only]
Scaffold:   agents-toolkit new [template-name]
Hosting:    [Azure Container Apps | App Service | None (declarative)]
Channels:   [M365 Copilot, Teams, Web, etc.]

Connectors & Data:
  - [Connector/MCP server 1] — [what it provides]
  - [Connector/MCP server 2] — [what it provides]

Why this approach:
  [2-3 sentences explaining the reasoning — why this type, why this framework,
   why these connectors. Mention what alternatives exist and why you didn't
   recommend them.]
```

Wait for the user to confirm or adjust before proceeding.

Consult `references/path-selection-matrix.md` for the decision logic behind recommendations.

---

## Step 3: Build the Plan

Once confirmed, produce a focused plan. This plan also becomes the source for the
**project CLAUDE.md** that gets generated during scaffolding (see platform-builder skill).

```
AGENT PLAN: [Agent Name]
━━━━━━━━━━━━━━━━━━━━━━━

1. SCAFFOLD
   Command:  agents-toolkit new [template] --lang [language]
   Creates:  [brief description of the project structure]

2. CONFIGURE
   - Agent instructions (system prompt / instructions.md)
   - Knowledge sources: [list]
   - Actions/Tools: [list with brief description of each]
   - Manifest settings: [key configurations]

3. CONNECT
   - [Data source 1]: via [MCP server | OpenAPI plugin | connector]
   - [Data source 2]: via [method]
   - Authentication: [Entra ID setup needed]

4. TEST
   - Local: Agents Toolkit test harness / Agents Playground
   - Integration: [specific test scenarios]

5. DEPLOY
   - Target: [Azure Container Apps | App Service | Copilot sideload]
   - CI/CD: [GitHub Actions | Azure DevOps]
   - Governance: Agent 365 registration

ENVIRONMENT NEEDED:
  [List of VS Code extensions, CLIs, MCP servers, and licenses required.
   The platform-builder skill will check these and help install what's missing.]
```

Read `references/agent-plan-template.md` for the full plan template with all fields.

---

## Step 4: Execute — Hand Off to Platform Builder

After plan approval, hand off to the **platform-builder** skill for execution. That skill:

1. **Checks the environment** — detects installed extensions, CLIs, and MCP servers.
   Recommends and helps configure anything missing. If the user isn't comfortable with
   a technology (Azure containers, Foundry, etc.), it recommends MCP servers that can
   automate those parts.
2. **Scaffolds via Agents Toolkit** — runs the template command, not custom file generation.
3. **Generates the project CLAUDE.md** — captures all plan decisions so any AI assistant
   (Claude Code, GitHub Copilot) understands the project context in future sessions.
4. **Guides configuration** — walks through agent instructions, connectors, tools, and testing.
5. **Validates the build** — smoke tests, integration checks, governance registration.

### Execution Modes

The user chooses how they want to work:

- **Guided (default)** — Walk through each step. Explain what's happening and why.
  Teach as you go. This is pair programming, not a tutorial dump.
- **Autonomous** — Generate and execute everything. Present a summary when done.
  Best for experienced users who just want working code.
- **Hybrid** — Auto-generate boilerplate (scaffold, infra, deployment). Guide through
  custom logic (agent instructions, tools, connectors).

Read `references/execution-modes.md` for detailed guidance on each mode.

---

## Agents Toolkit — The Default Scaffolding Tool

The Microsoft Agents Toolkit (VS Code extension + CLI) is the standard way to create,
test, and deploy agents for the Microsoft platform.

### What the Toolkit Handles

- Project scaffolding with correct structure and dependencies
- Local debugging and testing (Agents Playground)
- Provisioning Azure resources
- Sideloading to Teams / M365 for testing
- Publishing to org catalog or store
- App manifest generation and validation

### What the Toolkit Doesn't Handle (This Plugin Fills the Gap)

- **Choosing the right template** — that's this skill's job
- **Configuring agent instructions** — we guide effective prompt writing
- **Setting up connectors and data sources** — we map requirements to integrations
- **Infrastructure beyond the agent** — MCP servers, custom APIs, data pipelines
- **Governance setup** — Agent 365 registration, Entra configuration
- **Project CLAUDE.md** — so AI assistants understand the project in future sessions
- **Environment setup** — detecting and configuring required extensions, CLIs, MCP servers

### Key Templates

Consult `references/project-templates.md` for the full Agents Toolkit template catalog,
including exact scaffold commands and what each template produces. Use
`microsoft_docs_search` to verify current template names — they update frequently.

---

## Agent Type Spectrum

```
No Code ◄──────────────────────────────────────────────► Full Pro-Code

SharePoint    Agent       Copilot      Copilot Studio   Declarative     Custom Engine
  Agent      Builder      Studio Lite     Full          Agent (Toolkit)  Agent (SDK)
                                                                           ▲
                                                                     Azure AI Foundry
                                                                     (backend hosting)
```

### Building Path References

- **Zero-Code** — SharePoint Agent & Agent Builder. Read `references/zero-code-agents.md`.
- **Low-Code** — Copilot Studio. Read `references/copilot-studio-guide.md`.
- **Pro-Code Declarative** — Agents Toolkit. Read `references/declarative-agents-guide.md`.
- **Pro-Code Custom Engine** — Agents SDK. Read `references/custom-engine-agents-guide.md`.
- **Azure AI Foundry** — Managed hosting. Read `references/foundry-guide.md`.

---

## Orchestration Framework Selection

For custom engine agents that need orchestration beyond basic tool-calling:

| Framework | Best For | Integration |
|-----------|----------|-------------|
| **Semantic Kernel** | Microsoft-native, function calling, memory, planners | First-class — Microsoft maintains it |
| **LangChain / LangGraph** | Complex reasoning chains, large tool ecosystem | Supported — Python and JS |
| **Agents SDK only** | Simple tool-calling, no complex orchestration | Built-in — no extra dependency |

Read `references/orchestration-frameworks.md` for detailed comparison.

---

## Protocols for Agent Interoperability

- **MCP** (Model Context Protocol) — Agent-to-tool communication
- **A2A** (Agent-to-Agent) — Multi-agent coordination and delegation
- **OpenAPI** — Standard REST API integration

Read `references/protocols-guide.md` for implementation patterns.

---

## Agent Review and Improvement

When reviewing an existing agent (or one just built), assess these dimensions.
Read `references/review-checklist.md` for the detailed checklist.

**Quick Review — 5 Questions:**

1. **Instructions** — Does the agent know its role, boundaries, and fallback behavior?
2. **Knowledge** — Does it have access to everything it needs, and nothing it shouldn't?
3. **Actions** — Can it DO things, or does it just talk? Could it do more?
4. **Security** — Least privilege? Registered with Agent 365? Content guardrails?
5. **UX** — Conversation starters? Right tone? Rich responses where appropriate?

---

## Using Microsoft Learn MCP

Always use `microsoft_docs_search` and `microsoft_docs_fetch` to verify:
- Current Agents Toolkit templates and CLI commands
- SDK versions, API signatures, and manifest schemas
- Feature availability (GA vs preview vs deprecated)
- Licensing requirements (change frequently)

Do not guess at syntax or template names — look them up.

---

## Important Documentation Links

- **Agents Toolkit**: https://learn.microsoft.com/en-us/microsoft-365/developer/overview-m365-agents-toolkit
- **Agents SDK**: https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/agents-sdk-overview
- **Declarative Agents**: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent
- **Custom Engine Agents**: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-custom-engine-agent
- **Copilot Studio**: https://learn.microsoft.com/en-us/microsoft-copilot-studio/fundamentals-what-is-copilot-studio
- **Agent 365**: https://learn.microsoft.com/en-us/microsoft-agent-365/overview
- **Foundry Agent Service**: https://learn.microsoft.com/en-us/azure/foundry/agents/overview
- **Agents SDK Samples**: https://github.com/microsoft/Agents
- **Agent 365 Samples**: https://github.com/microsoft/Agent365-Samples

---

## Reference Files

- **`references/path-selection-matrix.md`** — Decision logic for agent type selection
- **`references/agent-plan-template.md`** — Full agent design plan template
- **`references/execution-modes.md`** — Guided vs autonomous vs hybrid build details
- **`references/project-templates.md`** — Agents Toolkit template catalog and scaffold commands
- **`references/zero-code-agents.md`** — SharePoint Agent and Agent Builder guide
- **`references/copilot-studio-guide.md`** — Copilot Studio development guide
- **`references/declarative-agents-guide.md`** — Declarative agents with Agents Toolkit
- **`references/custom-engine-agents-guide.md`** — Custom engine agents, SDK, A2A, MCP
- **`references/foundry-guide.md`** — Azure AI Foundry agent development
- **`references/infrastructure-requirements.md`** — Docker, Python, Bicep, Azure setup
- **`references/orchestration-frameworks.md`** — Semantic Kernel, LangChain, etc.
- **`references/protocols-guide.md`** — A2A, MCP, and OpenAPI integration patterns
- **`references/review-checklist.md`** — Agent review and improvement checklist
