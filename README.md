# claude-marketplace

Brandon Marcurella's personal [Claude Code](https://claude.ai/claude-code) plugin marketplace.

## Plugins

### ms-copilot-platform
Comprehensive guide to the Microsoft Frontier Agent Platform. Covers:
- Platform architecture (10-layer stack, component selection)
- Agent development (Copilot Studio, Declarative Agents, Custom Engine Agents, Agents SDK, Azure AI Foundry)
- Connectors & data (MCP servers, Power Platform, Microsoft Graph)
- Governance (Agent 365, Entra, Purview, Defender, Copilot Analytics)
- Labs & demos (quickstarts, hands-on tutorials, sample code)

**Command:** `/refresh-platform-docs [all|architecture|agents|connectors|governance|labs]`

### example-plugin
A template plugin with a sample skill and command. Use this as a starting point when creating new plugins.

## Installing this Marketplace in Claude Code

Open Claude Code settings and add this marketplace:

- **Source:** GitHub
- **Repo:** `bmarcurella/claude-marketplace`

Or via the Claude Desktop app's marketplace manager.

## Adding a New Plugin

1. Copy `plugins/example-plugin` → `plugins/your-plugin-name`
2. Update `.claude-plugin/plugin.json` with your plugin's name, description, and author
3. Edit `skills/` and `commands/` as needed
4. Add your plugin to `.claude-plugin/marketplace.json`
5. Commit and push — Claude will pick up the update on next marketplace refresh
