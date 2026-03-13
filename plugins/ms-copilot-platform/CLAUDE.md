# Microsoft Copilot Platform Plugin

This plugin guides the development, architecture, governance, and deployment of AI agents
on the Microsoft Frontier Agent Platform. It's designed for use with Claude Code and
GitHub Copilot in VS Code.

## How This Plugin Works

The plugin has five skills that work together across the agent lifecycle:

### Skill Relationships

```
User describes what they want
        ↓
[agent-development]  — Listens, infers, recommends agent type and plan
        ↓
[platform-builder]   — Checks environment, scaffolds via Agents Toolkit,
                       generates project CLAUDE.md, builds infrastructure
        ↓
[agent-governance]   — Registers with Agent 365, configures security and monitoring
```

Supporting skills (used when needed):
- **platform-architecture** — Explains how Microsoft platform components fit together.
  Use when designing complex solutions or understanding the full stack.
- **connectors-data** — Guides connecting agents to enterprise data sources, MCP servers,
  and other agents via A2A/MCP/OpenAPI protocols.

### Default Approach: Agents Toolkit

All pro-code agents are scaffolded using the **Microsoft Agents Toolkit** (VS Code extension
+ CLI). This provides:
- Consistent project structure across agent types
- Templates maintained by Microsoft with current SDK versions
- Built-in provisioning, testing, and deployment
- Git-friendly project layout

The plugin adds value on top of the Toolkit:
- Choosing the right template based on requirements
- Configuring agent instructions, tools, and connectors
- Setting up infrastructure (Bicep, Docker, CI/CD)
- Generating project CLAUDE.md for AI assistant context
- Detecting and configuring MCP servers for automation
- Agent 365 governance and monitoring setup

### Non-Toolkit Paths

For simpler needs, the plugin redirects to:
- **SharePoint Agent** — zero-code Q&A over SharePoint documents
- **Agent Builder** — zero-code custom Copilot in M365
- **Copilot Studio** — low-code visual builder with 1000+ connectors

## Key Concepts

- **Declarative Agent** — Extends M365 Copilot with custom instructions, knowledge, and
  actions. No hosting required. Built via Agents Toolkit manifest files.
- **Custom Engine Agent** — Full control over AI model, orchestration, and hosting.
  Built with M365 Agents SDK in Python, C#, or TypeScript.
- **Agents Toolkit** — VS Code extension and CLI for scaffolding, testing, and deploying
  both agent types.
- **MCP Servers** — Provide tool-calling capabilities to agents (and to AI assistants
  during development for automating Azure, GitHub, etc.)
- **Agent 365** — Enterprise governance layer for agent registration, security, and monitoring.

## Project CLAUDE.md Convention

Every agent project scaffolded by this plugin includes a CLAUDE.md at the project root.
This file captures architecture decisions, key files, commands, connectors, and conventions
so that Claude Code and GitHub Copilot have full context when working in the project.

## Refreshing Documentation

The Microsoft agent ecosystem changes frequently. Use the `/refresh-platform-docs` command
to pull the latest documentation from Microsoft Learn and update the plugin's reference files.

## Important Links

- Agents Toolkit: https://learn.microsoft.com/en-us/microsoft-365/developer/overview-m365-agents-toolkit
- Agents SDK: https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/agents-sdk-overview
- Agent 365: https://learn.microsoft.com/en-us/microsoft-agent-365/overview
- Copilot Extensibility: https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/agents-overview
- SDK Samples: https://github.com/microsoft/Agents
