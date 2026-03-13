# Project CLAUDE.md Template

This template is used to generate a CLAUDE.md file at the root of every agent project
scaffolded by the platform-builder skill. The CLAUDE.md gives any AI assistant (Claude Code,
GitHub Copilot, or any LLM-based tool) full context about the project.

## When to Generate

Generate the CLAUDE.md immediately after scaffolding (Phase 2), before configuring the agent.
Update it after each subsequent phase as the project evolves.

## Template

Populate the sections below from the approved plan. Remove sections that don't apply.
Keep it concise — this is a reference document, not documentation.

---

```markdown
# [Agent Name]

[One-sentence description of what this agent does and who it's for.]

## Architecture

- **Agent type:** [Declarative | Custom Engine]
- **Language:** [Python | C# | TypeScript]
- **Framework:** [Semantic Kernel | LangChain | Agents SDK only | N/A]
- **Hosting:** [Azure Container Apps | App Service | Azure Functions | None (declarative)]
- **Channels:** [M365 Copilot, Teams, Web, etc.]
- **Scaffolded with:** `agents-toolkit new [template] --lang [language]`

### Why these choices
[2-3 sentences from the plan explaining why this agent type, framework, and hosting
were chosen over alternatives.]

## Key Files

| File | Purpose |
|------|---------|
| `[manifest file]` | Agent manifest — defines capabilities and metadata |
| `[instructions file]` | System prompt — agent personality, rules, boundaries |
| `[main handler]` | Core agent logic — handles messages and tool calls |
| `[tools directory]` | Custom tools/functions the agent can invoke |
| `infra/` | Bicep templates for Azure resource provisioning |
| `.env.example` | Environment variables template (copy to .env) |

## Connectors and Data Sources

| Source | Method | Purpose |
|--------|--------|---------|
| [Source 1] | [MCP server / OpenAPI plugin / Graph API / connector] | [What data it provides] |
| [Source 2] | [method] | [purpose] |

### Authentication
[How auth is configured — Entra app registration, managed identity, API keys in Key Vault, etc.]

## Commands

```bash
# Local development
[command to start locally — e.g., docker-compose up, python app.py, dotnet run]

# Run tests
[test command]

# Deploy to Azure
[deploy command — e.g., agents-toolkit deploy, ./scripts/deploy.sh]

# Provision infrastructure
[infra command — e.g., az deployment group create, agents-toolkit provision]

# Clean up / tear down
[cleanup command — e.g., ./scripts/cleanup.sh]
```

## MCP Servers Configured

[List any MCP servers configured for this project's development workflow]

| Server | What It Automates |
|--------|-------------------|
| [MCP server name] | [what it does in this project's context] |

## Environment Variables

[List the key environment variables from .env.example with descriptions]

| Variable | Description |
|----------|-------------|
| `AZURE_OPENAI_ENDPOINT` | Azure OpenAI service endpoint |
| `AZURE_OPENAI_DEPLOYMENT` | Model deployment name |
| [etc.] | [etc.] |

## Conventions

- **Error handling:** [How errors are handled — e.g., tool failures return user-friendly messages]
- **Testing:** [Testing approach — e.g., pytest with fixtures for each tool]
- **Secrets:** All secrets stored in Azure Key Vault, referenced via environment variables
- **Branching:** [Git branching strategy if defined]

## Governance

- **Agent 365 registered:** [Yes/No — registration ID if yes]
- **Entra app ID:** [app registration ID]
- **Monitoring:** [Application Insights resource, Copilot Analytics dashboard]
- **Data classification:** [What sensitivity level the agent handles]
```

---

## Guidelines for Populating

1. **Be specific.** "See the docs" is not helpful. Include the actual file paths, commands,
   and configuration values.

2. **Explain the why.** The "Why these choices" section is critical — it prevents future
   developers (or AI assistants) from second-guessing architecture decisions without context.

3. **Keep it current.** Update the CLAUDE.md when:
   - New connectors or tools are added
   - Infrastructure changes
   - Environment variables are added or changed
   - Deployment process changes

4. **Don't duplicate docs.** The CLAUDE.md is an index and context file, not a full
   README. Point to the README for detailed setup instructions.

5. **Include MCP servers.** If MCP servers are configured for the project's dev workflow,
   list them. This tells future AI sessions what automation is available.
