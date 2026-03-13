# Agent Project Scaffolds

Quick reference for generating project scaffolds. Maps directly to the templates
in `agent-development/references/project-templates.md`.

## Scaffold Selection

| Design Choice | Template | Command |
|--------------|----------|---------|
| Custom Engine + Python + SK | T1 | `teamsfx new --app-type custom-engine-agent --programming-language python --ai-service semantic-kernel` |
| Custom Engine + Python + LC | T2 | Manual scaffold (see project-templates.md T2) |
| Custom Engine + C# + SK | T3 | `teamsfx new --app-type custom-engine-agent --programming-language csharp --ai-service semantic-kernel` |
| Custom Engine + TS + SK | T4 | `teamsfx new --app-type custom-engine-agent --programming-language typescript --ai-service semantic-kernel` |
| Declarative Agent | T5 | `teamsfx new --app-type declarative-agent` |
| Copilot Studio | T6 | Export via Power Platform CLI |
| MCP Server | T7 | Manual scaffold (see project-templates.md T7) |
| Multi-Agent A2A | T8 | Manual scaffold (see project-templates.md T8) |

## Post-Scaffold Customization

After generating the scaffold:
1. Rename project and update all references
2. Configure environment variables
3. Replace placeholder tools with domain-specific tools
4. Update manifest.json with correct app details
5. Customize Bicep templates for required Azure resources
6. Add domain-specific tests

## Scaffold Verification

After generating, verify:
```bash
# Check structure
ls -la src/ infra/ appPackage/ tests/

# Verify dependencies install
pip install -r requirements.txt  # Python
# npm install                    # Node.js
# dotnet restore                 # C#

# Verify Bicep compiles
az bicep build --file infra/main.bicep

# Run tests
pytest tests/  # Python
```
