# Agent Development Path Selection Matrix

Detailed decision matrix for choosing the right agent-building approach on the Microsoft
Frontier Agent Platform.

## Quick Decision Flow

```
What does the agent need to do?
│
├─ Answer questions from documents only?
│  ├─ Documents in SharePoint? → SharePoint Agent (zero code)
│  ├─ Need custom instructions? → Agent Builder (zero code)
│  └─ Need connectors or triggers? → Copilot Studio
│
├─ Automate a business process?
│  ├─ Standard connectors available? → Copilot Studio
│  ├─ Custom API integration? → Declarative Agent + API Plugin
│  └─ Complex orchestration? → Custom Engine Agent
│
├─ Custom AI behavior or model?
│  ├─ Custom instructions only? → Declarative Agent
│  ├─ Custom orchestration? → Custom Engine Agent
│  └─ Fine-tuned model? → Foundry + Custom Engine
│
├─ Multi-channel deployment?
│  ├─ M365 + Teams only? → Declarative Agent or Copilot Studio
│  └─ Web + email + SMS? → Custom Engine Agent (Agents SDK)
│
├─ Agent-to-agent coordination?
│  └─ Always → Custom Engine Agent with A2A
│
└─ Autonomous/proactive behavior?
   ├─ Time-based triggers? → Copilot Studio or Custom Engine
   └─ Event-driven autonomous? → Custom Engine Agent
```

## Detailed Comparison Matrix

| Capability | SharePoint Agent | Agent Builder | Copilot Studio | Declarative Agent | Custom Engine |
|-----------|:---:|:---:|:---:|:---:|:---:|
| **Coding Required** | None | None | Low-code | JSON + API | Full code |
| **Setup Time** | 2 min | 5 min | Hours | Hours-Days | Days-Weeks |
| **Knowledge Sources** | SharePoint | SharePoint, Web | SharePoint, Web, Dataverse, Custom | SharePoint, Graph, Web, API | Any |
| **Custom Instructions** | No | Basic | Advanced | Advanced | Full control |
| **API Actions** | No | No | Via connectors | Via API plugins | Full control |
| **Power Automate** | No | No | Yes | No (use API instead) | Via integration |
| **Adaptive Cards** | No | No | Yes | Yes | Yes |
| **Multi-channel** | Copilot only | Copilot only | Multi-channel | Copilot + Teams | Any channel |
| **Autonomous triggers** | No | No | Yes | No | Yes |
| **A2A (agent-to-agent)** | No | No | Limited | No | Yes |
| **Custom AI model** | No | No | No | No | Yes |
| **Computer Use** | No | No | No | No | Via Foundry |
| **Hosting** | Microsoft | Microsoft | Microsoft | Microsoft | Azure / Self |
| **Source control** | No | No | Yes (export) | Yes (native) | Yes (native) |
| **Agent 365 governed** | Yes | Yes | Yes | Yes | Yes |

## Licensing Requirements

### No Additional License
- SharePoint Agent: Included with M365 Copilot
- Agent Builder: Included with M365 Copilot

### M365 Copilot License Required
- Declarative Agents: Included with M365 Copilot license
- Some basic features work without Copilot license

### Copilot Studio License Required
- Copilot Studio agents: Separate license or included in some M365 plans
- Message-based consumption billing
- Microsoft 365 Copilot Chat included (no Copilot license needed for Teams-only)

### Azure Consumption
- Custom Engine Agents: Azure hosting costs (compute, AI models, storage)
- Foundry: Azure AI Foundry workspace costs
- Cost depends on usage volume and model selection

### Agent 365
- GA: May 1, 2026
- Included with qualifying M365 plans or $15/user/month standalone
- Required for enterprise governance features

## The "Graduate Up" Pattern

Start simple and move up as needs grow:

### Level 1: Prototype (Minutes)
Tool: SharePoint Agent or Agent Builder
Goal: Prove the concept works with minimal effort
Output: Working agent answering questions from documents

### Level 2: Enhance (Hours-Days)
Tool: Copilot Studio
Goal: Add connectors, workflows, triggers, richer interactions
Output: Business-ready agent with integrations

### Level 3: Professionalize (Days-Weeks)
Tool: Declarative Agent via Agents Toolkit (VS Code)
Goal: Source control, CI/CD, API plugins, Adaptive Cards
Output: Developer-maintained agent with full lifecycle

### Level 4: Scale (Weeks)
Tool: Custom Engine Agent + Foundry + Agent 365
Goal: Custom orchestration, multi-channel, enterprise governance
Output: Enterprise-grade agent solution

## Common Anti-Patterns

### Over-Engineering
Building a custom engine agent when a declarative agent would suffice.
**Signal:** The agent only needs custom instructions and document retrieval.
**Fix:** Start with declarative agent; upgrade only when hitting limits.

### Under-Engineering
Forcing Copilot Studio to do things it wasn't designed for.
**Signal:** Complex custom logic in Power Automate flows, fighting against the platform.
**Fix:** Evaluate custom engine for complex orchestration scenarios.

### Ignoring Governance
Building agents without Agent 365 registration.
**Signal:** "Shadow agents" that IT doesn't know about.
**Fix:** Register every agent, even prototypes, with Agent 365.

### Single-Channel Thinking
Designing for Teams only when users need multi-channel access.
**Signal:** Users asking "can I use this in Outlook/web/email?"
**Fix:** Plan multi-channel from the start; choose a path that supports it.
