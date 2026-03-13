# Microsoft Frontier Agent Platform — Detailed Layer Breakdown

This reference provides comprehensive descriptions of every component in each platform layer,
sourced from the Microsoft Frontier Agent Platform architecture.

## Layer 1: The UI for AI — Microsoft 365 Copilot

Microsoft 365 Copilot is "the front door for agents." It provides a unified interface where
users interact with agents in Teams, Outlook, Office apps, and on any device. Capabilities
include: chat, search, create, reason, and act — all in one experience.

M365 Copilot is not just a chatbot — it's the orchestration layer that routes user requests
to the appropriate agent, manages context, and presents results.

## Layer 2: Agent Store

Three categories of agents available through the store:

### 3rd Party Agents
Agents built by ISVs and partners. Examples include:
- **SAP Joule** — SAP's AI copilot for enterprise processes
- **Illuminate** — Knowledge management agent
- **NowAssist** — ServiceNow's AI assistant

### Microsoft Agents
First-party agents built into Microsoft products:
- **Outlook Agent** — Email management and scheduling
- **Teams Agent** — Meeting summaries and channel management
- **Word Agent** — Document creation and editing
- **Excel Agent** — Data analysis and formula generation
- **PowerPoint Agent** — Presentation creation
- **Researcher Agent** — Web research and synthesis
- **Self-Service Agent** — IT help desk
- **Fabric Agent** — Data analytics

### Custom Agents
Agents built by the organization:
- **Copilot Studio** agents — Low-code custom agents
- **Foundry Agents** — Pro-code Azure-hosted agents
- **Agent Framework** agents — Open Agent SDK agents
- **Gemini** agents — Third-party LLM agents (multi-model support)

## Layer 3: Agent Hosting / Orchestration

Four orchestration services that host and run agents:

### Microsoft 365 Copilot Agent Orchestration
Hosts declarative agents and manages the Copilot experience. Handles:
- Declarative agent manifest interpretation
- Knowledge source grounding
- API plugin invocation
- Adaptive Card rendering

### Microsoft Copilot Studio Agent Orchestration
Managed hosting for Copilot Studio agents. Provides:
- Topic-based conversation management
- Power Automate flow execution
- Connector integration
- Multi-channel publishing

### Microsoft Foundry Agent Orchestration
Azure-hosted agent runtime. Provides:
- Custom model hosting (Azure OpenAI, open-source models)
- Semantic Kernel / LangChain orchestration
- Stateful agent sessions
- Scale-to-zero and auto-scaling

### Microsoft Windows 365 Computer Using Agents
Agents that can operate a full Windows desktop environment:
- UI automation via computer vision
- Application interaction
- Legacy system integration

### Capabilities Block
Common capabilities across hosting services:
- **Models** — LLM model selection and configuration
- **Fine-tuning** — Custom model fine-tuning
- **Evaluations** — Agent performance evaluation
- **Workflows** — Multi-step workflow orchestration
- **Computer Use** — UI automation capabilities
- **Responsible AI** — Safety filters and content moderation

## Layer 4: Intelligence

Three IQ services that provide intelligence capabilities:

### Work IQ
Workplace intelligence powered by Microsoft Graph signals:
- User activity patterns and preferences
- Collaboration network analysis
- Document relevance scoring
- Meeting context and follow-up tracking

### Fabric IQ
Data intelligence powered by Microsoft Fabric:
- Semantic model understanding
- Natural language to SQL translation
- Data quality assessment
- Automated insight generation

### Foundry IQ
AI model intelligence:
- Model routing and selection
- Prompt optimization
- Response quality assessment
- Multi-model orchestration

### Intelligence Capabilities
- **Work-memory** — Persistent context across sessions
- **Personalisation** — User-specific behavior adaptation
- **Fine-tuning** — Domain-specific model optimization
- **Inferencing** — Model serving and response generation
- **Orchestration** — Multi-step reasoning and tool use
- **Actions** — Real-world action execution

## Layer 5: Open Protocols

Three open standards for interoperability:

### OpenAPI
Standard REST API specification format. Used for:
- Defining API plugins for declarative agents
- Exposing custom APIs to Copilot
- Cross-platform API integration

### MCP (Model Context Protocol)
Standardized protocol for agent-tool communication:
- Tool discovery and invocation
- Context sharing between agent and tools
- Structured input/output schemas
- Stateful tool sessions

### A2A (Agent-to-Agent Protocol)
Protocol for multi-agent communication:
- Agent discovery and capability exchange
- Task delegation between agents
- Collaborative problem solving
- Cross-platform agent orchestration

## Layer 6: MCP Servers

Microsoft's first-party MCP server implementations:

### Outlook MCP Server
- Read/write emails, calendar events, contacts
- Search mail and calendar
- Create drafts and send messages
- Manage folders and categories

### Teams MCP Server
- Read/write channel messages
- Access meeting transcripts and recordings
- Manage team membership
- Search across Teams content

### Search MCP Server
- Microsoft Search across all M365 content
- Graph Search for entity resolution
- Web search integration
- Content ranking and relevance

### Fabric MCP Server
- Query semantic models and lakehouses
- Execute SQL against data warehouses
- Access real-time analytics
- Browse data catalog

### D365 MCP Server
- CRUD operations on Dynamics 365 entities
- Business process automation
- Customer data access
- Financial and operations data

## Layer 7: Connectors

Pre-built enterprise system connectors:
- **D365** — Dynamics 365 CRM and F&O
- **Salesforce** — CRM, Sales, Service, Marketing clouds
- **SAP** — S/4HANA, BTP, ERP modules
- **Workday** — HCM, Finance, Planning, Payroll
- **ServiceNow** — ITSM, ITOM, HR Service Delivery
- **Dataverse** — Microsoft's low-code data platform

## Layer 8: Enterprise Data Platform

### Azure Databricks
- Lakehouse architecture (Delta Lake)
- Spark-based data processing
- ML model training and serving
- Unity Catalog for governance

### Microsoft Fabric
- OneLake (unified data lake)
- Data warehouse and lakehouse
- Real-time analytics
- Data engineering (pipelines, dataflows)
- Power BI integration

### Microsoft Dataverse
- Relational data storage for business apps
- Virtual tables for external data
- Business rules and calculated fields
- Row-level security
- Power Platform integration backbone

### Data Platform Capabilities
- **Warehouse** — SQL-based analytics
- **Real-time** — Streaming data processing
- **Low-code** — Visual data transformation
- **Data-flows** — ETL/ELT pipelines
- **Semantic-models** — Business logic abstraction
- **BI-integration** — Power BI and reporting

## Layer 9: Enterprise Systems of Record

The foundational business applications:
- **D365 CRM** — Customer relationship management
- **D365 F&O** — Finance and Operations ERP
- **Salesforce** — External CRM
- **SAP** — External ERP
- **Workday** — External HCM/Finance
- **ServiceNow** — External ITSM
- **SQL Databases** — Custom relational databases
- **Storage** — Azure Blob, Data Lake, file storage

## Layer 10: Agent Control System (Side Panel)

### Agent 365
Unified agent governance:
- **Entra** — Agent identity and access management
- **Purview** — Security, data governance, observability
- **Defender** — Threat protection and security monitoring
- **Admin** — Agent registry, tools, usage data

### Copilot Analytics
Performance monitoring and insights for all agents.

### Operations
- **Assets** — Agents, Models, Tools inventory
- **Compliance** — Policies, Guardrails enforcement
- **Quota** — TPM, PTU, Rate-limiting management
- **Admin** — Projects, AI Gateway configuration

### All Agents
Cross-platform agent visibility:
- Copilot Studio agents
- Foundry agents
- Open AI agents
- Gemini agents
- AWS Bedrock agents
- Marketplace agents

## Agent Development Tooling (Left Panel)

### Power Platform
- **Copilot Studio Lite** — Zero-code agents
- **Copilot Studio Full** — Natural language / low-code agents
- **Power Automate** — AI-infused workflow/RPA
- **Power Apps** — AI-enabled apps

### Azure AI
- **Foundry** — Agent Service (managed agent hosting)
- **Logic Apps** — AI-enabled workflow

### Agents Toolkit
- **GitHub Copilot** — Agent Mode (code generation assistance)
- **M365 Agents SDK** — Teams, M365 Copilot SDK
- **Agent 365 SDK** — Identity, Observability SDK
- **Agent Framework** — Open Agent SDK
