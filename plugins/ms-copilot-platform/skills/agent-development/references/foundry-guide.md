# Azure AI Foundry Guide

Azure AI Foundry provides managed hosting for enterprise-grade agents with built-in
model management, evaluations, responsible AI tooling, and Azure-native infrastructure.

## What Foundry Provides

- **Agent Service** — Managed hosting and scaling for agents
- **Model Catalog** — Access to Azure OpenAI, open-source, and partner models
- **Evaluations** — Automated quality, safety, and performance testing
- **Fine-tuning** — Custom model training on organizational data
- **Responsible AI** — Content safety, groundedness checks, PII detection
- **Monitoring** — Application Insights integration, token tracking

## When to Choose Foundry

Choose when: enterprise-scale hosting, model evaluation pipelines, fine-tuning,
or Azure-managed agent infrastructure is needed. Especially valuable for:
- Teams that want managed compute without handling containers directly
- Scenarios requiring model comparison and evaluation
- Agents that need fine-tuned models
- Enterprise compliance requirements (managed infrastructure)

## Architecture

```
Azure AI Foundry Hub
├── AI Project (your agent project)
│   ├── Agent Definition
│   ├── Model Deployments (GPT-4o, GPT-4o-mini, etc.)
│   ├── Tool Connections (Bing, SharePoint, Azure AI Search, etc.)
│   ├── Evaluations
│   └── Monitoring
├── Shared Resources
│   ├── Azure OpenAI Service
│   ├── Azure AI Search
│   ├── Key Vault
│   └── Storage Account
└── Identity & Access (Entra ID)
```

## Getting Started

### Create a Hub and Project
```bash
# Via Azure CLI
az ml workspace create --kind hub --name aih-myproject --resource-group rg-agents
az ml workspace create --kind project --name aip-myagent --hub-name aih-myproject
```

### Deploy a Model
```bash
az ml online-deployment create \
  --name gpt4o-deployment \
  --model azureml://registries/azureml/models/gpt-4o/versions/latest \
  --instance-type Standard_DS3_v2
```

### Create an Agent with Foundry Agent Service

```python
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

client = AIProjectClient(
    credential=DefaultAzureCredential(),
    endpoint="https://aip-myagent.services.ai.azure.com"
)

agent = client.agents.create_agent(
    model="gpt-4o",
    name="Contoso Support Agent",
    instructions="You are a helpful support agent for Contoso.",
    tools=[
        {"type": "code_interpreter"},
        {"type": "file_search"},
        {"type": "function", "function": {...}}
    ]
)
```

## Foundry + Agents SDK Integration

Foundry hosts the agent backend; the Agents SDK handles channel communication:

```
Teams/Copilot → Agents SDK (channel layer) → Foundry Agent Service (AI layer)
```

This separation means:
- Foundry handles model orchestration, tool calling, state management
- Agents SDK handles Teams/Copilot/web channel protocols
- Scaling and infrastructure is Azure-managed

## Evaluations Pipeline

Test agent quality automatically:

```python
evaluation = client.evaluations.create(
    data="test-dataset.jsonl",
    evaluators={
        "relevance": RelevanceEvaluator(model_config),
        "groundedness": GroundednessEvaluator(model_config),
        "coherence": CoherenceEvaluator(model_config),
        "safety": ContentSafetyEvaluator()
    }
)
```

## Cost Considerations

- Foundry Hub: Free (management layer)
- Compute: Pay for model inference (token-based)
- Storage: Standard Azure storage rates
- AI Search: Depends on tier selected
- Monitoring: Application Insights ingestion rates

## Best Practices

1. Start with a project, not just standalone deployments
2. Set up evaluations before going to production
3. Use managed identity for all service connections
4. Enable content safety filters
5. Monitor token consumption and set budgets
6. Use Foundry's built-in tools before building custom ones
7. Always verify current SDK versions via `microsoft_docs_search`
