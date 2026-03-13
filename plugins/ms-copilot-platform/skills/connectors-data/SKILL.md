---
name: connectors-data
description: >
  This skill should be used when the user asks about "MCP servers for Microsoft", "connect my
  agent to Outlook", "connect to Teams", "connect to Salesforce", "connect to SAP",
  "Microsoft Graph connectors", "Dataverse integration", "Microsoft Fabric", "enterprise data
  platform", "connect agent to data", "A2A protocol", "Model Context Protocol", "OpenAPI
  integration", "agent connectors", "D365 connector", "Workday connector", "ServiceNow connector",
  or any question about connecting agents to enterprise data sources, systems of record, MCP
  servers, open protocols, or the enterprise data platform. Also covers Azure Databricks, Fabric IQ,
  Work IQ, semantic models, data flows, and BI integration.
---

# Connectors, Data & Open Protocols

This skill covers everything that connects agents to data, services, and each other — the
middleware layers of the Microsoft Frontier Agent Platform.

## Three Integration Paradigms

### 1. Open Protocols

The platform supports three open protocols for interoperability:

- **OpenAPI** — Standard REST API definitions. Agents call APIs via OpenAPI specs.
- **MCP** (Model Context Protocol) — Standardized agent-to-tool communication. Agents discover
  and invoke tools exposed by MCP servers.
- **A2A** (Agent-to-Agent) — Protocol for multi-agent orchestration. Agents discover, communicate
  with, and delegate to other agents.

Read `references/open-protocols.md` for detailed protocol specifications and usage patterns.

### 2. Microsoft MCP Servers

Microsoft provides first-party MCP servers for core services:

| MCP Server | Capabilities | Use Cases |
|-----------|-------------|-----------|
| **Outlook** | Email, calendar, contacts | Email triage, scheduling, contact lookup |
| **Teams** | Messages, channels, meetings | Team coordination, meeting summaries |
| **Search** | Microsoft Search, Graph Search | Enterprise knowledge retrieval |
| **Fabric** | Data lakehouse, semantic models | Analytics, BI queries, data exploration |
| **D365** | CRM, ERP, business data | Customer data, order management, finance |

Read `references/mcp-servers.md` for connection setup, authentication, and tool schemas.

### 3. Enterprise Connectors

Pre-built connectors for enterprise systems:

- **D365** — Dynamics 365 CRM and F&O
- **Salesforce** — CRM, Sales Cloud, Service Cloud
- **SAP** — ERP, S/4HANA, BTP
- **Workday** — HCM, Finance, Planning
- **ServiceNow** — ITSM, ITOM, SecOps
- **Dataverse** — Microsoft's low-code data platform

Read `references/enterprise-connectors.md` for connector setup and best practices.

## Enterprise Data Platform

The data foundation for intelligent agents:

### Azure Databricks
Unified analytics platform. Use for large-scale data processing, ML model training,
and lakehouse architecture. Agents can query Databricks via SQL endpoints or APIs.

### Microsoft Fabric
End-to-end analytics platform. Key capabilities for agents:
- **Warehouse** — SQL-based data warehousing
- **Real-time** — Streaming data and event processing
- **Low-code** — Visual data transformation
- **Data-flows** — ETL/ELT pipelines
- **Semantic-models** — Business logic layer for analytics
- **BI-integration** — Power BI reports and dashboards

### Microsoft Dataverse
Structured business data store. The backbone of Power Platform and D365.
Agents interact via Dataverse APIs or the Dataverse connector.

## Intelligence Layer

The platform's AI services that make agents smarter:

- **Work IQ** — Workplace intelligence (user patterns, collaboration signals)
- **Fabric IQ** — Data intelligence (semantic understanding of enterprise data)
- **Foundry IQ** — AI model intelligence (orchestration, routing, evaluations)

Intelligence capabilities: work-memory, personalization, fine-tuning, inferencing,
orchestration, and actions.

## Integration Design Workflow

### Step 1: Identify Data Sources
Map what data the agent needs and where it lives.

### Step 2: Choose Integration Method
- **MCP** for tool-like interactions (read/write with context)
- **OpenAPI** for standard REST API calls
- **Connectors** for pre-built enterprise integrations
- **Graph API** for M365 data (users, mail, files, calendar)
- **Dataverse** for Power Platform business data

### Step 3: Configure Authentication
Most integrations require OAuth 2.0 or managed identity. Entra ID is the identity backbone.

### Step 4: Test and Validate
Verify data access, error handling, rate limits, and data transformation.

## Reference Files

- **`references/open-protocols.md`** — OpenAPI, MCP, and A2A protocol details
- **`references/mcp-servers.md`** — Microsoft's first-party MCP servers
- **`references/enterprise-connectors.md`** — Connector setup for D365, Salesforce, SAP, etc.
- **`references/data-platform.md`** — Azure Databricks, Fabric, and Dataverse guide
- **`references/intelligence-layer.md`** — Work IQ, Fabric IQ, and Foundry IQ details
