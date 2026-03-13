# Architecture Diagram Templates

Reusable Mermaid diagram templates for common agent solution architectures.

## Template 1: Simple Q&A Agent

```mermaid
graph TD
    User[User] -->|Chat| Copilot[M365 Copilot]
    Copilot -->|Route| DA[Declarative Agent]
    DA -->|Ground| KS[Knowledge Sources]
    KS --> SP[SharePoint Library]
    KS --> Web[Web Content]
    DA -->|Governed by| A365[Agent 365]
    A365 --> Entra[Entra ID]
    A365 --> Purview[Purview DLP]
```

## Template 2: Business Process Agent

```mermaid
graph TD
    User[User] -->|Chat| Copilot[M365 Copilot]
    Copilot -->|Route| CS[Copilot Studio Agent]
    CS -->|Topics| NLU[Natural Language Understanding]
    CS -->|Actions| PA[Power Automate Flow]
    PA -->|Connect| D365[Dynamics 365]
    PA -->|Connect| SF[Salesforce]
    CS -->|Knowledge| DV[Dataverse]
    CS -->|Governed by| A365[Agent 365]
```

## Template 3: Pro-Code Custom Engine Agent

```mermaid
graph TD
    User[User] -->|Chat| Copilot[M365 Copilot]
    User -->|Chat| Teams[Teams]
    User -->|Chat| Web[Web App]
    Copilot --> SDK[M365 Agents SDK]
    Teams --> SDK
    Web --> SDK
    SDK --> Orch[Custom Orchestration]
    Orch --> LLM[Azure OpenAI]
    Orch --> Tools[Tool Functions]
    Tools -->|MCP| OutlookMCP[Outlook MCP Server]
    Tools -->|MCP| TeamsMCP[Teams MCP Server]
    Tools -->|API| CustomAPI[Custom APIs]
    SDK -->|Hosted on| Foundry[Azure AI Foundry]
    SDK -->|Governed by| A365[Agent 365]
```

## Template 4: Multi-Agent System

```mermaid
graph TD
    User[User] -->|Chat| Copilot[M365 Copilot]
    Copilot -->|Route| Router[Agent Router]
    Router -->|A2A| Agent1[HR Agent]
    Router -->|A2A| Agent2[IT Agent]
    Router -->|A2A| Agent3[Finance Agent]
    Agent1 -->|MCP| TeamsMCP[Teams MCP]
    Agent2 -->|MCP| SearchMCP[Search MCP]
    Agent3 -->|MCP| D365MCP[D365 MCP]
    Agent1 --> CS[Copilot Studio]
    Agent2 --> Foundry[Azure AI Foundry]
    Agent3 --> SDK[Agents SDK]
    A365[Agent 365] -.->|Governs| Agent1
    A365 -.->|Governs| Agent2
    A365 -.->|Governs| Agent3
```

## Template 5: Enterprise Data Intelligence Agent

```mermaid
graph TD
    User[User] -->|Chat| Copilot[M365 Copilot]
    Copilot -->|Route| Agent[Data Agent]
    Agent -->|Intelligence| FIQ[Fabric IQ]
    FIQ -->|Query| Fabric[Microsoft Fabric]
    Fabric --> Warehouse[Data Warehouse]
    Fabric --> Lakehouse[Lakehouse]
    Fabric --> Semantic[Semantic Models]
    Agent -->|MCP| FabricMCP[Fabric MCP Server]
    Agent -->|Connect| Databricks[Azure Databricks]
    Fabric --> PBI[Power BI Dashboards]
    Agent -->|Governed by| A365[Agent 365]
```

## Template 6: Full Enterprise Architecture

```mermaid
graph TD
    subgraph UI["UI Layer"]
        Copilot[M365 Copilot]
        Teams[Teams]
        Outlook[Outlook]
        Web[Web/Mobile]
    end

    subgraph Agents["Agent Layer"]
        DA[Declarative Agents]
        CSA[Copilot Studio Agents]
        CEA[Custom Engine Agents]
    end

    subgraph Hosting["Hosting Layer"]
        M365Orch[M365 Orchestration]
        CSOrch[Copilot Studio Orchestration]
        FOrch[Foundry Orchestration]
    end

    subgraph Intelligence["Intelligence Layer"]
        WIQ[Work IQ]
        FIQ[Fabric IQ]
        FOIQ[Foundry IQ]
    end

    subgraph Protocols["Protocol Layer"]
        MCP[MCP Servers]
        A2A[A2A Protocol]
        OAPI[OpenAPI]
    end

    subgraph Data["Data Layer"]
        Fabric[Microsoft Fabric]
        DV[Dataverse]
        DB[Azure Databricks]
    end

    subgraph Systems["Systems of Record"]
        D365[D365]
        SF[Salesforce]
        SAP[SAP]
        SN[ServiceNow]
    end

    subgraph Control["Control System"]
        A365[Agent 365]
        Analytics[Copilot Analytics]
        Ops[Operations]
    end

    UI --> Agents
    Agents --> Hosting
    Hosting --> Intelligence
    Hosting --> Protocols
    Protocols --> Data
    Data --> Systems
    Control -.->|Governs| Agents
    Control -.->|Monitors| Hosting
```

## Usage Notes

- Customize by replacing placeholder names with actual component names
- Add or remove nodes based on the specific solution
- Use solid arrows (-->) for data/request flow
- Use dashed arrows (-.->) for governance/monitoring relationships
- Use subgraphs to group related components
- Color-code by layer when rendering (UI=blue, Agents=green, Data=purple, Control=red)
