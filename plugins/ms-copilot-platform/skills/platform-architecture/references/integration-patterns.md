# Integration Patterns Between Platform Layers

Common patterns for connecting components across the Microsoft Frontier Agent Platform.

## Pattern 1: Copilot → Declarative Agent → SharePoint (Simple Retrieval)

**When to use:** Agent answers questions from organizational documents.
**Flow:** User query → Copilot → Declarative agent manifest routes to knowledge → SharePoint Graph connector retrieves content → LLM generates grounded response.
**Key config:** Declarative agent manifest with `capabilities.OneDriveAndSharePoint` knowledge source.

## Pattern 2: Copilot → Copilot Studio → Power Automate → Enterprise System

**When to use:** Agent needs to read/write data in enterprise systems (D365, Salesforce, SAP).
**Flow:** User query → Copilot → Copilot Studio topic triggers → Power Automate flow executes → Connector accesses enterprise system → Response flows back.
**Key config:** Copilot Studio topic with Power Automate action, connector authentication.

## Pattern 3: Copilot → Custom Engine → MCP Servers

**When to use:** Agent needs real-time access to M365 data via standardized tool protocol.
**Flow:** User query → Copilot → Custom engine agent → MCP client discovers tools → MCP server executes tool → Data returned to agent → LLM generates response.
**Key config:** MCP server endpoint configuration, OAuth for MCP server auth, tool schemas.

## Pattern 4: Agent → A2A → Multiple Agents

**When to use:** Complex request that spans multiple domains (HR + IT + Finance).
**Flow:** User query → Router agent → A2A protocol discovers relevant agents → Tasks delegated → Sub-agents execute → Results aggregated → Unified response.
**Key config:** A2A agent cards, capability descriptions, task routing logic.

## Pattern 5: Agent → Fabric IQ → Microsoft Fabric

**When to use:** Agent needs data analytics, BI insights, or natural language data queries.
**Flow:** User query → Agent → Fabric IQ translates to SQL/DAX → Fabric executes against semantic model → Results returned → Agent formats response with charts/tables.
**Key config:** Fabric workspace connection, semantic model configuration, Fabric MCP server.

## Pattern 6: Agent → Azure AI Foundry → Custom Model

**When to use:** Agent needs specialized AI model (fine-tuned, domain-specific, or non-OpenAI).
**Flow:** User query → Agent → Foundry orchestration → Custom model inference → Post-processing → Response with responsible AI checks.
**Key config:** Foundry workspace, model deployment, evaluation pipeline, content safety.

## Pattern 7: Full Enterprise (All Layers)

**When to use:** Enterprise-grade agent solution touching all platform layers.
**Flow:**
1. User interacts via M365 Copilot (UI Layer)
2. Request routes to appropriate agent (Agent Store)
3. Agent hosted on Foundry/Copilot Studio (Hosting Layer)
4. Work IQ provides personalization (Intelligence Layer)
5. Agent calls tools via MCP (Protocol Layer)
6. Data retrieved from Fabric/Dataverse (Data Platform)
7. Enterprise systems queried as needed (Systems of Record)
8. Agent 365 governs the entire flow (Control System)

## Cross-Cutting Integration Concerns

### Authentication Flow
Every integration needs authentication. Standard pattern:
1. User authenticates to M365 via Entra ID
2. Agent inherits user context (delegated permissions)
3. Agent uses managed identity for service-to-service calls
4. MCP servers validate tokens via Entra
5. Connectors use stored credentials or OAuth flows

### Error Handling Pattern
1. Timeout handling (MCP servers, APIs)
2. Retry with exponential backoff
3. Fallback to alternative data sources
4. Graceful degradation (partial results vs. failure)
5. Error reporting to Copilot Analytics

### Data Flow Classification
- **Real-time**: MCP servers, Graph API, live API calls
- **Near-real-time**: Dataverse, Power Automate triggers
- **Batch**: Fabric pipelines, scheduled data refreshes
- **Cached**: SharePoint indexing, Graph connector crawl
