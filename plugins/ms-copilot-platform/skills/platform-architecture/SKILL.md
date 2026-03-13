---
name: platform-architecture
description: >
  Use when the user asks about the Microsoft Frontier Agent Platform architecture, how
  platform components fit together, solution architecture design, component selection,
  integration patterns, or the 10-layer platform model (Copilot UI, Agent Store, hosting,
  intelligence, protocols, MCP servers, connectors, data platform, systems of record,
  Agent Control System).
---

# Microsoft Frontier Agent Platform Architecture

This skill provides comprehensive knowledge of the Microsoft Frontier Agent Platform — the full
stack that powers AI agents across the Microsoft ecosystem. The platform has 10 interconnected
layers, and understanding how they fit together is essential for designing effective agent solutions.

## Platform Layers Overview

The Frontier Agent Platform is organized into these interconnected layers. Consult
`references/platform-layers.md` for detailed descriptions of each component.

### Layer Map (Top to Bottom)

1. **The UI for AI** — Microsoft 365 Copilot as the front door for all agents
2. **Agent Store** — 3rd party agents, Microsoft agents, and custom agents
3. **Agent Hosting / Orchestration** — M365 Copilot, Copilot Studio, Foundry, Windows 365
4. **Intelligence** — Work IQ, Fabric IQ, Foundry IQ (personalization, memory, orchestration)
5. **Open Protocols** — OpenAPI, MCP, A2A for interoperability
6. **MCP Servers** — Outlook, Teams, Search, Fabric, D365 (Microsoft's first-party MCP servers)
7. **Connectors** — D365, Salesforce, SAP, Workday, ServiceNow, Dataverse
8. **Enterprise Data Platform** — Azure Databricks, Microsoft Fabric, Dataverse
9. **Enterprise Systems of Record** — D365 CRM, D365 F&O, Salesforce, SAP, Workday, ServiceNow, SQL, Storage
10. **Agent Control System** — Agent 365, Copilot Analytics, Operations (side panel)

### Agent Development Tooling (Left Panel)

The development side spans a spectrum from no-code to full pro-code:

- **Power Platform**: Copilot Studio Lite, Copilot Studio Full, Power Automate, Power Apps
- **Azure AI**: Foundry Agent Service, Logic Apps
- **Agents Toolkit**: GitHub Copilot (Agent Mode), M365 Agents SDK, Agent 365 SDK, Agent Framework

## Architecture Design Workflow

When helping design a solution architecture:

### Step 1: Gather Requirements

Identify the scenario, user personas, data sources, integration points, compliance needs, and
scale expectations. Use `references/requirements-template.md` for a structured intake.

### Step 2: Map to Platform Layers

For each requirement, identify which platform layer(s) are involved. A typical agent solution
touches 4-6 layers. Consult `references/platform-layers.md` for component capabilities.

### Step 3: Select Components

Choose specific components within each layer. Common patterns include:

| Pattern | Components | Best For |
|---------|-----------|----------|
| Simple Q&A Agent | Copilot UI → Agent Builder → SharePoint | Document retrieval, FAQ |
| Business Process Agent | Copilot UI → Copilot Studio → Power Automate → D365 | Workflow automation |
| Pro-Code Agent | Copilot UI → Agents SDK → Foundry → Azure AI | Custom orchestration |
| Multi-Agent System | Copilot UI → Custom Engine → A2A → Multiple Agents | Complex workflows |
| Data Intelligence Agent | Copilot UI → Fabric IQ → Microsoft Fabric → Dataverse | Analytics, BI |

### Step 4: Design Integration Points

Map how components communicate. Key integration protocols:
- **MCP** (Model Context Protocol) — Agent-to-tool communication
- **A2A** (Agent-to-Agent) — Multi-agent orchestration
- **OpenAPI** — REST API integration
- **Microsoft Graph** — M365 data access
- **Dataverse** — Structured business data

### Step 5: Address Cross-Cutting Concerns

For every architecture, address:
- **Identity & Access**: Entra ID, least-privilege, conditional access
- **Security**: Purview for data governance, Defender for threat protection
- **Compliance**: Guardrails, policies, audit trails
- **Monitoring**: Copilot Analytics, operational dashboards
- **Quota & Rate Limiting**: TPM, PTU, rate-limiting considerations

### Step 6: Create Architecture Artifacts

Generate visual architecture diagrams using Mermaid or draw.io format.
Use `references/diagram-templates.md` for reusable architecture diagram patterns.

## Using Microsoft Learn MCP for Latest Information

Always verify architecture details against current documentation. Use `microsoft_docs_search`
and `microsoft_docs_fetch` to:
- Confirm component names and capabilities (the ecosystem evolves rapidly)
- Check GA/preview/deprecated status of features
- Verify integration patterns and supported protocols
- Retrieve current pricing and licensing models

## Reference Files

- **`references/platform-layers.md`** — Detailed breakdown of all 10 platform layers with every component
- **`references/requirements-template.md`** — Structured requirements gathering template
- **`references/diagram-templates.md`** — Reusable Mermaid architecture diagram templates
- **`references/integration-patterns.md`** — Common integration patterns between layers
