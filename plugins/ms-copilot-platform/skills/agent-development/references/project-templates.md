# Agents Toolkit Templates & Scaffold Reference

The Microsoft Agents Toolkit provides project templates for both declarative and custom engine
agents. Always use `agents-toolkit new` to scaffold — do NOT generate project files manually.
The Toolkit templates stay current with SDK versions and include correct dependencies.

## Important: Verify Before Scaffolding

Template names and options change with Toolkit updates. Before running any scaffold command,
use `microsoft_docs_search` to verify the current template catalog:
- Search: "Agents Toolkit project templates"
- Search: "agents-toolkit new command options"

## Agents Toolkit Templates

### Declarative Agent Templates

| Template | What It Creates | Use When |
|----------|----------------|----------|
| **declarative-agent** | M365 Copilot agent with custom instructions and knowledge | Agent extends Copilot with custom behavior, no hosting needed |
| **declarative-agent-with-api** | Declarative agent + OpenAPI plugin | Agent needs to call external REST APIs |
| **declarative-agent-with-connector** | Declarative agent + Copilot connector | Agent needs external data via Microsoft Graph connector |

**Scaffold commands:**
```bash
# Basic declarative agent
agents-toolkit new declarative-agent --name "my-agent"

# Declarative agent with API plugin
agents-toolkit new declarative-agent-with-api --name "my-agent"

# Declarative agent with connector
agents-toolkit new declarative-agent-with-connector --name "my-agent"
```

**What the Toolkit generates:**
```
my-agent/
├── appPackage/
│   ├── manifest.json              # Teams/M365 app manifest
│   ├── declarativeAgent.json      # Agent definition (instructions, capabilities)
│   ├── instruction.md             # System prompt (agent personality and rules)
│   ├── outline.png                # App icon (outline)
│   └── color.png                  # App icon (color)
├── env/                           # Environment files per stage
├── teamsapp.yml                   # Agents Toolkit lifecycle config
└── teamsapp.local.yml             # Local debug config
```

### Custom Engine Agent Templates

| Template | Language | Framework | Use When |
|----------|----------|-----------|----------|
| **custom-engine-agent** | Python | Agents SDK | Full control over AI model and orchestration |
| **custom-engine-agent** | C# | Agents SDK | .NET-based custom engine |
| **custom-engine-agent** | TypeScript | Agents SDK | Node.js-based custom engine |

**Scaffold commands:**
```bash
# Python custom engine
agents-toolkit new custom-engine-agent --lang python --name "my-agent"

# C# custom engine
agents-toolkit new custom-engine-agent --lang csharp --name "my-agent"

# TypeScript custom engine
agents-toolkit new custom-engine-agent --lang typescript --name "my-agent"
```

**What the Toolkit generates (Python example):**
```
my-agent/
├── src/
│   ├── app.py                     # Agent entry point
│   ├── config.py                  # Configuration/settings
│   └── bot.py                     # Activity handler
├── appPackage/
│   ├── manifest.json              # Teams/M365 app manifest
│   ├── outline.png
│   └── color.png
├── infra/                         # Bicep templates for Azure
│   ├── azure.bicep
│   └── azure.parameters.json
├── env/
├── requirements.txt
├── Dockerfile
├── teamsapp.yml
└── teamsapp.local.yml
```

### Other Templates

The Toolkit may offer additional templates for specific scenarios. Check current availability:
```bash
agents-toolkit new --list
```

## What the Toolkit Handles vs What We Add

| Concern | Toolkit Provides | This Plugin Adds |
|---------|-----------------|------------------|
| Project structure | Base scaffold with correct dependencies | — |
| Agent manifest | Manifest template | Customized metadata and capabilities |
| Agent instructions | Empty/minimal instruction.md | Crafted system prompt with role, boundaries, fallback |
| Tools/actions | Skeleton handler | Domain-specific tool implementations |
| Infrastructure | Basic Bicep templates | Additional modules (Key Vault, monitoring, MCP servers) |
| Testing | Local debug config | Test scenarios, integration tests |
| CI/CD | — | GitHub Actions / Azure DevOps pipelines |
| CLAUDE.md | — | Full project context for AI assistants |
| Docker | Basic Dockerfile | docker-compose for local dev with MCP servers |
| Governance | — | Agent 365 registration, Entra config |

## Supplementary Templates (Not from Agents Toolkit)

These templates cover scenarios the Agents Toolkit doesn't scaffold. The platform-builder
skill generates these when the plan requires them.

### MCP Server (Python + FastMCP)

For when the agent needs custom tools exposed via MCP:

```
{mcp-server-name}/
├── mcp_server.py              # MCP server with tool definitions
├── requirements.txt           # fastmcp + dependencies
├── Dockerfile                 # Container for deployment
├── docker-compose.yml         # Local dev alongside the agent
├── .env.example
├── infra/
│   └── container-app.bicep    # Azure Container Apps deployment
├── scripts/
│   ├── deploy.sh
│   └── cleanup.sh
└── tests/
    └── test_tools.py
```

### Multi-Agent System (A2A)

For when multiple agents coordinate via Agent-to-Agent protocol:

```
{system-name}/
├── router/                    # Orchestrator agent
│   ├── src/
│   │   ├── app.py
│   │   ├── router_agent.py
│   │   └── agent_registry.py
│   ├── requirements.txt
│   └── Dockerfile
├── agents/                    # Specialist agents
│   ├── {agent-1}/
│   │   ├── src/
│   │   ├── agent-card.json    # A2A agent card for discovery
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   └── {agent-2}/
│       └── ... (same structure)
├── docker-compose.yml         # Runs all agents locally
├── infra/
│   ├── main.bicep
│   └── modules/
│       ├── container-apps-env.bicep
│       ├── router-app.bicep
│       └── agent-app.bicep
├── scripts/
│   ├── deploy-all.sh
│   └── cleanup-all.sh
└── CLAUDE.md                  # System-level project context
```

## Adding Orchestration Frameworks

After scaffolding with the Toolkit, add an orchestration framework if the plan calls for one:

### Adding Semantic Kernel
```bash
# Python
pip install semantic-kernel

# C#
dotnet add package Microsoft.SemanticKernel
```

Then modify the agent handler to use Kernel for orchestration.
See `references/orchestration-frameworks.md` for integration patterns.

### Adding LangChain
```bash
# Python
pip install langchain langchain-openai langgraph
```

See `references/orchestration-frameworks.md` for LangChain + Agents SDK integration.

## Template Selection Logic

Use this decision tree when choosing a template:

1. Does the agent need its own AI model or custom orchestration?
   - **No** → Declarative agent template
   - **Yes** → Custom engine template

2. (If declarative) Does it call external APIs?
   - **No** → `declarative-agent`
   - **Yes, REST APIs** → `declarative-agent-with-api`
   - **Yes, via Graph connector** → `declarative-agent-with-connector`

3. (If custom engine) What language?
   - Python (default for AI/data users) → `--lang python`
   - C# (default for .NET shops) → `--lang csharp`
   - TypeScript (default for web devs) → `--lang typescript`

4. Does it need MCP server tools? → Add supplementary MCP server template
5. Does it coordinate with other agents? → Add supplementary multi-agent template
