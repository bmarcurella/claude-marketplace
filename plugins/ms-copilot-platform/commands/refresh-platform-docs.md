---
description: Pull latest Microsoft Frontier Agent Platform documentation from Microsoft Learn and update skill reference files.
argument-hint: "[all|architecture|agents|connectors|governance|labs]"
---

# Refresh Platform Documentation

This command pulls the latest documentation from Microsoft Learn using the `microsoft_docs_search`
and `microsoft_docs_fetch` MCP tools, then updates the skill reference files with current information.

## Execution Steps

### 1. Determine Scope

Based on the `scope` argument, determine which documentation areas to refresh:

| Scope | Areas Refreshed |
|-------|----------------|
| `all` | All areas below |
| `architecture` | Platform layers, component capabilities, integration patterns |
| `agents` | Agent types, SDK versions, manifest schemas, Copilot Studio features |
| `connectors` | MCP servers, connectors, protocols, data platform |
| `governance` | Agent 365, Entra, Purview, Defender, analytics |
| `labs` | Quickstarts, tutorials, sample code, environment setup |

### 2. Fetch Latest Documentation

For each area, search Microsoft Learn for the most current content:

#### Architecture Refresh Queries
- "Microsoft Frontier Agent Platform architecture"
- "Microsoft 365 Copilot extensibility overview"
- "Microsoft agent hosting orchestration"
- "Work IQ Fabric IQ Foundry IQ"

#### Agent Development Refresh Queries
- "Copilot Studio create agent latest"
- "Declarative agent manifest schema"
- "M365 Agents SDK overview"
- "Azure AI Foundry agent service"
- "Custom engine agent development"
- "Agent Builder M365 Copilot"

#### Connectors & Data Refresh Queries
- "Microsoft MCP servers Outlook Teams"
- "A2A agent-to-agent protocol Microsoft"
- "Microsoft Fabric agent integration"
- "Power Platform connectors catalog"
- "Microsoft Graph API agents"

#### Governance Refresh Queries
- "Agent 365 overview governance"
- "Microsoft Entra agent identity"
- "Microsoft Purview AI governance"
- "Copilot Analytics monitoring"
- "Microsoft Defender AI agents"

#### Labs Refresh Queries
- "Microsoft 365 Copilot agent quickstart"
- "Copilot Studio tutorial getting started"
- "Agents SDK sample code"
- "Azure AI Foundry agent tutorial"

### 3. Compare and Update

For each search result:
1. Fetch the full page content using `microsoft_docs_fetch`
2. Compare key facts against current reference file content
3. Identify what has changed (new features, deprecated features, schema changes, etc.)
4. Update the relevant reference files with current information
5. Add a timestamp to track when the reference was last updated

### 4. Report Changes

After refreshing, provide a summary:

```
Platform Documentation Refresh Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Scope: [scope]
Date: [timestamp]

Updated Files:
  ✓ references/platform-layers.md — Updated Foundry IQ capabilities
  ✓ references/copilot-studio-guide.md — Added new autonomous trigger types
  ✓ references/mcp-servers.md — Updated Fabric MCP server tools

No Changes:
  · references/open-protocols.md — Already current
  · references/validation-rules.md — Already current

New Information Found:
  ★ New MCP server for SharePoint (preview)
  ★ Agent 365 GA date confirmed
  ★ New Agents SDK version 2.x released

Warnings:
  ⚠ Copilot Studio Lite renamed to "Copilot Studio Express"
  ⚠ Deprecated: BotFramework Composer (use Agents Toolkit instead)
```

### 5. Handling Errors

If Microsoft Learn MCP is rate-limited or unavailable:
1. Report which queries failed
2. Retry failed queries after a short delay
3. If still failing, report partial results and suggest trying again later

## Usage

```
/refresh-platform-docs                  # Refresh everything
/refresh-platform-docs scope=agents     # Refresh agent development docs only
/refresh-platform-docs scope=connectors # Refresh connector and data docs only
```
