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

This skill guides the development of AI agents across the full Microsoft platform spectrum.
Every engagement starts with a plan — no building until the user understands what, why, and how.

## Core Workflow: Plan First, Build Second

Every agent development engagement follows this mandatory flow:

### Step 1: Discovery — Understand the Goal

Before recommending anything, gather these essentials:

1. **Agent purpose** — What should the agent do? (Q&A, automate workflows, coordinate agents, etc.)
2. **Users** — Who interacts with it? (internal employees, customers, a specific team)
3. **Channels** — Where does it live? (M365 Copilot, Teams, web, email, multi-channel)
4. **Data sources** — What data does it need? (SharePoint, APIs, databases, CRM, ERP)
5. **Actions** — What should it DO beyond answering? (create tickets, send emails, update records)
6. **Autonomy** — Should it act proactively or only respond to users?
7. **Scale** — How many users? What volume? What SLAs?
8. **Technical context** — User's coding comfort, existing infrastructure, team capabilities

### Step 2: Present 3 Options with Pros/Cons

After discovery, ALWAYS present exactly 3 viable approaches ranked from simplest to most complex.
Each option includes: what it is, how it works, pros, cons, estimated effort, and cost implications.

**Template for each option:**

```
Option [N]: [Name] — [One-line summary]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Path:        [Agent type and tooling]
Effort:      [Time estimate]
Skill level: [No-code / Low-code / Pro-code]
Cost:        [Licensing and hosting summary]

How it works: [2-3 sentences]

Pros:
  + [Advantage 1]
  + [Advantage 2]
  + [Advantage 3]

Cons:
  - [Limitation 1]
  - [Limitation 2]

Infrastructure needed:
  [What Azure resources, licenses, tools are required]

Growth path:
  [Where this can go if needs evolve]
```

Consult `references/path-selection-matrix.md` for the detailed comparison data behind
these recommendations.

### Step 3: Design the Plan

Once the user selects an option, produce a structured **Agent Design Plan**:

```
Agent Design Plan: [Agent Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. OVERVIEW
   Purpose, users, channels, success criteria

2. ARCHITECTURE
   Diagram (Mermaid), component list, data flow

3. REQUIREMENTS
   Prerequisites:    [Licenses, subscriptions, permissions]
   Infrastructure:   [Azure resources, Docker, runtime environments]
   Development tools: [VS Code, SDKs, CLIs, extensions]
   Data access:      [Connectors, APIs, MCP servers needed]

4. BUILD SEQUENCE
   Phase 1: [Foundation — what gets built first and why]
   Phase 2: [Core agent — the agent itself]
   Phase 3: [Integrations — data, actions, connectors]
   Phase 4: [Polish — UX, error handling, testing]
   Phase 5: [Governance — Agent 365, monitoring, security]

5. ESTIMATED EFFORT & COST
   Build time, ongoing costs, licensing

6. EXECUTION MODE
   □ Walk me through it (learn how and why at each step)
   □ Build it for me (autonomous execution via Claude Code)
```

Read `references/agent-plan-template.md` for the full plan template with all fields.

### Step 4: Execute — Two Modes

After plan approval, ask how the user wants to proceed:

**Mode A: Guided Walkthrough (Default)**
Walk through each phase step-by-step. At every step explain:
- WHAT is being done
- WHY it matters (architecture context)
- HOW to do it (exact commands, files, configurations)
- VERIFY — how to confirm it worked
- LEARN — what concept this teaches

**Mode B: Autonomous Build (Claude Code in VS Code)**
Generate all artifacts and execute automatically. Produce:
- Complete project scaffold with all files
- Infrastructure templates (Bicep/ARM)
- Configuration files
- Deployment scripts
- README with architecture explanation
- Cleanup/teardown scripts

Read `references/execution-modes.md` for detailed guidance on both modes.

## Agent Development Spectrum

```
No Code ◄──────────────────────────────────────────────► Full Pro-Code

SharePoint    Agent       Copilot      Copilot Studio   Declarative     Custom Engine
  Agent      Builder      Studio Lite     Full          Agent (Toolkit)  Agent (SDK)
                                                                           ▲
                                                                     Azure AI Foundry
                                                                     (backend hosting)
```

## Building Paths — Quick Reference

### Zero-Code: SharePoint Agent & Agent Builder
Simple retrieval agents. No coding. Read `references/zero-code-agents.md`.

### Low-Code: Copilot Studio
Visual builder, 1000+ connectors, autonomous triggers, generative topics.
Read `references/copilot-studio-guide.md`.

### Pro-Code: Declarative Agents (Agents Toolkit)
JSON manifests extending M365 Copilot. VS Code, source control, API plugins.
Read `references/declarative-agents-guide.md`.

### Pro-Code: Custom Engine Agents (Agents SDK)
Full control — model, orchestration, hosting. Python, .NET, Node.js.
Supports A2A, MCP, LangChain, Semantic Kernel, any LLM.
Read `references/custom-engine-agents-guide.md`.

### Azure AI Foundry
Managed Azure hosting. Model management, evaluations, responsible AI.
Read `references/foundry-guide.md`.

## Infrastructure Requirements

Agent development often requires infrastructure beyond the agent code itself:

- **Docker** — Container hosting for MCP servers, custom APIs, agent runtimes
- **Python / Node.js / .NET** — Runtime environments for SDK-based agents
- **Bicep / ARM** — Infrastructure as Code for Azure resource provisioning
- **Azure CLI** — Resource management and deployment
- **Power Platform CLI** — Copilot Studio solution management

Read `references/infrastructure-requirements.md` for setup guides for each.

## Orchestration Frameworks

Custom engine agents need an orchestration framework:

| Framework | Strengths | Best For |
|-----------|----------|----------|
| **Semantic Kernel** | Microsoft-native, function calling, planners, memory | M365-integrated agents |
| **LangChain / LangGraph** | Large ecosystem, agent graphs, retrieval chains | Complex multi-step reasoning |
| **OpenAI Agents SDK** | Native OpenAI tool use, simple API | OpenAI-model-centric agents |
| **Crew.ai** | Multi-agent orchestration, role-based agents | Team-of-agents patterns |
| **AutoGen** | Multi-agent conversation, code execution | Research/experimentation |

Read `references/orchestration-frameworks.md` for comparison and integration patterns.

## Protocols for Agent Interoperability

- **A2A** (Agent-to-Agent) — Multi-agent coordination and delegation
- **MCP** (Model Context Protocol) — Agent-to-tool communication
- **OpenAPI** — Standard REST API integration

Read `references/protocols-guide.md` for implementation patterns for each.

## Using Microsoft Learn MCP

Always use `microsoft_docs_search` and `microsoft_docs_fetch` to verify:
- Current SDK versions, API signatures, and manifest schemas
- Feature availability (GA vs preview vs deprecated)
- Licensing requirements (change frequently)
- Code samples and quickstarts

## Important Documentation Links

- **Agent 365**: https://learn.microsoft.com/en-us/microsoft-agent-365/overview
- **Agents for M365 Copilot**: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/agents-overview
- **Declarative Agents**: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent
- **Custom Engine Agents**: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-custom-engine-agent
- **Agents Toolkit**: https://learn.microsoft.com/en-us/microsoft-365/developer/overview-m365-agents-toolkit
- **Agents SDK**: https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/agents-sdk-overview
- **Copilot Studio**: https://learn.microsoft.com/en-us/microsoft-copilot-studio/fundamentals-what-is-copilot-studio
- **Foundry Agent Service**: https://learn.microsoft.com/en-us/azure/foundry/agents/overview
- **Agents SDK Samples**: https://github.com/microsoft/Agents

## Reference Files

- **`references/path-selection-matrix.md`** — Detailed decision matrix for choosing agent type
- **`references/agent-plan-template.md`** — Full agent design plan template
- **`references/execution-modes.md`** — Guided walkthrough vs autonomous build details
- **`references/zero-code-agents.md`** — SharePoint Agent and Agent Builder guide
- **`references/copilot-studio-guide.md`** — Comprehensive Copilot Studio development
- **`references/declarative-agents-guide.md`** — Declarative agents with Agents Toolkit
- **`references/custom-engine-agents-guide.md`** — Custom engine agents, SDK, A2A, MCP
- **`references/foundry-guide.md`** — Azure AI Foundry agent development
- **`references/infrastructure-requirements.md`** — Docker, Python, Bicep, Azure setup
- **`references/orchestration-frameworks.md`** — Semantic Kernel, LangChain, etc.
- **`references/protocols-guide.md`** — A2A, MCP, and OpenAPI integration patterns
- **`references/project-templates.md`** — Reusable project scaffolds for each agent type
- **`references/review-checklist.md`** — Agent review and improvement checklist
