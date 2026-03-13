# Lab Templates

Structured templates for hands-on learning experiences across the Microsoft Frontier Agent Platform.

## Lab Template Structure

Every lab follows this structure:

```
Lab: [Title]
Level: [Beginner | Intermediate | Advanced]
Duration: [Estimated time]
Prerequisites: [What's needed]
Platform Components: [Which platform layers are involved]

1. Overview (What and Why)
2. Architecture Context (Where this fits)
3. Prerequisites Check
4. Step-by-Step Instructions
5. Checkpoints (Verify progress)
6. Extension Challenges
7. Cleanup
8. Key Takeaways
```

## Getting Started Labs

### Lab GS-1: Your First Agent (SharePoint Agent)
**Level:** Beginner | **Duration:** 15 minutes
**Components:** M365 Copilot → SharePoint Agent → SharePoint Library
**What you build:** An agent that answers questions from a SharePoint document library.
**Why it matters:** Demonstrates the fastest path from zero to working agent.
**Prerequisites:** M365 Copilot license, SharePoint site with documents

### Lab GS-2: Agent Builder Custom Agent
**Level:** Beginner | **Duration:** 30 minutes
**Components:** M365 Copilot → Agent Builder → Custom Instructions + Knowledge
**What you build:** A custom Copilot agent with specific instructions and multiple knowledge sources.
**Why it matters:** Shows how custom instructions shape agent behavior without any code.
**Prerequisites:** M365 Copilot license

## Copilot Studio Labs

### Lab CS-1: Basic Copilot Studio Agent
**Level:** Beginner | **Duration:** 1 hour
**Components:** Copilot Studio → Topics → Knowledge Sources
**What you build:** An agent with custom topics, generative AI, and SharePoint knowledge.
**Why it matters:** Introduces the Copilot Studio visual designer.

### Lab CS-2: Connectors and Actions
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Copilot Studio → Power Automate → Connectors
**What you build:** An agent that reads/writes CRM data via connectors and executes workflows.
**Why it matters:** Demonstrates real business process automation with agents.

### Lab CS-3: Autonomous Agent
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Copilot Studio → Triggers → Autonomous Actions
**What you build:** An agent that proactively monitors for conditions and takes action.
**Why it matters:** Shows the evolution from reactive chatbot to proactive agent.

## Declarative Agent Labs

### Lab DA-1: First Declarative Agent
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Agents Toolkit (VS Code) → Declarative Agent Manifest → M365 Copilot
**What you build:** A declarative agent with custom instructions and SharePoint knowledge.
**Why it matters:** Introduces pro-code agent development with source control.

### Lab DA-2: API Plugin Integration
**Level:** Intermediate | **Duration:** 3 hours
**Components:** Declarative Agent → OpenAPI Spec → Custom API
**What you build:** A declarative agent that calls external APIs via OpenAPI plugins.
**Why it matters:** Demonstrates how agents take actions through API integration.

### Lab DA-3: Adaptive Cards
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Declarative Agent → Adaptive Cards → Teams
**What you build:** An agent that renders rich interactive cards in conversations.
**Why it matters:** Shows professional-grade UX in agent responses.

## Custom Engine Labs

### Lab CE-1: SDK Setup and Hello World
**Level:** Advanced | **Duration:** 2 hours
**Components:** Agents SDK → Azure → Teams Channel
**What you build:** A basic custom engine agent deployed to Teams.
**Why it matters:** Establishes the foundation for full pro-code development.

### Lab CE-2: Custom Orchestration
**Level:** Advanced | **Duration:** 4 hours
**Components:** Agents SDK → Semantic Kernel → Azure OpenAI → Tools
**What you build:** An agent with custom orchestration logic, tool calling, and multi-turn.
**Why it matters:** Demonstrates full control over AI behavior and reasoning.

### Lab CE-3: MCP Server Integration
**Level:** Advanced | **Duration:** 3 hours
**Components:** Custom Engine → MCP Client → Outlook MCP + Teams MCP
**What you build:** An agent that uses Microsoft MCP servers for email and Teams access.
**Why it matters:** Shows the modern tool integration pattern via MCP.

### Lab CE-4: Multi-Agent with A2A
**Level:** Advanced | **Duration:** 4 hours
**Components:** Custom Engine → A2A Protocol → Multiple Agents
**What you build:** A multi-agent system where specialized agents coordinate via A2A.
**Why it matters:** Demonstrates enterprise-scale agent architecture.

## Foundry Labs

### Lab FO-1: Azure AI Foundry Agent Service
**Level:** Advanced | **Duration:** 3 hours
**Components:** Azure AI Foundry → Agent Service → Model Deployment
**What you build:** An agent hosted on Azure AI Foundry with managed compute.
**Why it matters:** Shows enterprise-grade agent hosting with Azure.

### Lab FO-2: Model Evaluation Pipeline
**Level:** Advanced | **Duration:** 2 hours
**Components:** Foundry → Evaluations → Test Datasets
**What you build:** An evaluation pipeline that measures agent quality.
**Why it matters:** Demonstrates responsible AI testing practices.

## Governance Labs

### Lab GOV-1: Agent 365 Setup
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Agent 365 → Entra → Agent Registry
**What you build:** Agent 365 configured with agent registration, identity, and policies.
**Why it matters:** Establishes enterprise governance foundation.

### Lab GOV-2: Monitoring and Analytics
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Copilot Analytics → Dashboards → Alerts
**What you build:** Monitoring dashboards with KPIs, alerts, and usage tracking.
**Why it matters:** Shows how to operate agents in production.

## Integration Labs

### Lab INT-1: Fabric Data Agent
**Level:** Intermediate-Advanced | **Duration:** 3 hours
**Components:** Agent → Fabric IQ → Microsoft Fabric → Semantic Model
**What you build:** An agent that queries enterprise data through Fabric.
**Why it matters:** Demonstrates data intelligence integration.

### Lab INT-2: Enterprise Connector Agent
**Level:** Intermediate | **Duration:** 2 hours
**Components:** Copilot Studio → Connectors → D365/Salesforce
**What you build:** An agent connected to enterprise CRM data.
**Why it matters:** Shows real-world enterprise integration.

## Capstone Labs

### Lab CAP-1: Full Enterprise Agent Solution
**Level:** Advanced | **Duration:** 8 hours (full day)
**Components:** All platform layers
**What you build:** Complete enterprise agent solution with multiple agents, MCP integration,
Foundry hosting, and Agent 365 governance.
**Why it matters:** Puts it all together in a production-grade architecture.

## Lab Delivery Notes

- Always use `microsoft_docs_search` to pull the latest quickstart steps
- Verify UI paths and menu options before generating lab instructions
- Include screenshots or descriptions of expected UI states
- Provide both "happy path" and troubleshooting guidance
- Generate starter files that learners can copy and customize
