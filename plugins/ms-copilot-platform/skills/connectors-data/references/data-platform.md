# Enterprise Data Platform Guide

Detailed guide for Azure Databricks, Microsoft Fabric, and Dataverse integration with agents.

## Azure Databricks
Unified analytics platform for large-scale data processing.

### Agent Integration Patterns
- **SQL Endpoint** — Agents query Databricks via SQL
- **REST API** — Programmatic access to data and jobs
- **MCP Server** — Custom MCP server wrapping Databricks APIs

## Microsoft Fabric
End-to-end analytics platform with key capabilities:

### Data Warehouse
SQL-based warehousing. Agents query via SQL endpoints or Fabric REST API.

### Real-time Analytics
Streaming data processing. Agents can subscribe to event streams.

### Semantic Models
Business logic layer. Agents use Fabric IQ to query semantic models
in natural language — the model translates to DAX/SQL automatically.

### Data Flows
ETL/ELT pipelines. Agents can trigger data refresh or check pipeline status.

## Microsoft Dataverse
Structured business data store powering Power Platform and D365.

### Agent Integration
- **Dataverse connector** — Pre-built Power Platform connector
- **Web API** — RESTful CRUD operations
- **Dataverse MCP Server** — Microsoft's first-party MCP server

### Best Practices
1. Use semantic models for analytical queries (not raw tables)
2. Implement row-level security for agent data access
3. Cache frequently accessed data to reduce API calls
4. Monitor query performance and optimize as needed
