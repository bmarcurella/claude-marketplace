# Enterprise Connectors Guide

Setup and best practices for connecting agents to enterprise systems of record.

## D365 (Dynamics 365)
- **CRM** — Accounts, contacts, opportunities, cases
- **F&O** — Finance, supply chain, manufacturing
- **Connection:** Dataverse connector, D365 MCP Server, or Web API
- **Auth:** Entra ID with Dataverse permissions

## Salesforce
- **Data:** Accounts, contacts, opportunities, cases, custom objects
- **Connection:** Power Platform Salesforce connector or REST API
- **Auth:** OAuth 2.0 with Salesforce connected app

## SAP
- **Data:** ERP, S/4HANA, BTP services
- **Connection:** SAP connector (Power Platform), OData, RFC
- **Auth:** SAP credentials or SSO via Entra ID

## Workday
- **Data:** HCM, Finance, Planning
- **Connection:** Workday connector or REST API
- **Auth:** OAuth 2.0 or API key

## ServiceNow
- **Data:** Incidents, changes, requests, CMDB
- **Connection:** ServiceNow connector or REST API
- **Auth:** OAuth 2.0 or basic auth

## Best Practices
1. Use pre-built connectors when available (less code, maintained by vendor)
2. Implement retry logic for enterprise API calls
3. Cache reference data to reduce external calls
4. Respect rate limits on all external systems
5. Use managed identity or Key Vault for credentials
6. Test with realistic data volumes
