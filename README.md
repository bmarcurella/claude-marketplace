# bmarcurella-marketplace

Brandon Marcurella's personal plugin marketplace for [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli) and [Claude Code](https://claude.ai/claude-code).

## Plugins

### ms-copilot-platform

Comprehensive guide to the Microsoft Frontier Agent Platform. Covers:

- Platform architecture (10-layer stack, component selection)
- Agent development (Copilot Studio, Declarative Agents, Custom Engine Agents, Agents SDK, Azure AI Foundry)
- Connectors & data (MCP servers, Power Platform, Microsoft Graph)
- Governance (Agent 365, Entra, Purview, Defender, Copilot Analytics)
- Labs & demos (quickstarts, hands-on tutorials, sample code)

**Command:** `/refresh-platform-docs [all|architecture|agents|connectors|governance|labs]`

## Installation

### GitHub Copilot CLI

```bash
copilot plugin marketplace add bmarcurella/claude-marketplace
copilot plugin install ms-copilot-platform@bmarcurella-claude-marketplace
```

### Claude Code

Open Claude Code settings and add this marketplace:

- **Source:** GitHub
- **Repo:** `bmarcurella/claude-marketplace`

Or via the Claude Desktop app's marketplace manager.

## Adding a New Plugin

1. Create a new directory under `plugins/your-plugin-name`
2. Add a `plugin.json` manifest (for Copilot CLI) and `.claude-plugin/plugin.json` (for Claude Code)
3. Add skills in `skills/`, agents in `agents/`, commands in `commands/` as needed
4. Register the plugin in both `.github/plugin/marketplace.json` and `.claude-plugin/marketplace.json`
5. Commit and push — changes are picked up on next marketplace refresh
