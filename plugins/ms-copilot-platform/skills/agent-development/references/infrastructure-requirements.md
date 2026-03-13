# Infrastructure Requirements Guide

Everything needed to set up the development and hosting environment for agent solutions
on the Microsoft Frontier Agent Platform. Covers local development, Azure hosting,
Docker containers, runtime environments, and Infrastructure as Code.

## Development Environment Setup

### VS Code Extensions (Required)
- **Microsoft 365 Agents Toolkit** — Agent scaffolding, debugging, deployment
- **Azure Account** — Azure subscription management
- **Bicep** — Infrastructure as Code authoring
- **Docker** — Container management
- **Python / C# / Node.js** — Language-specific extensions

### CLI Tools (Required)
```bash
# Azure CLI — resource management and deployment
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az login

# Node.js (for JS agents and Agents Toolkit)
# Recommended: use nvm for version management
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
nvm install 20

# Python (for Python agents)
# Recommended: use pyenv for version management
# Minimum Python 3.10 for Agents SDK
python3 --version  # Verify >= 3.10

# .NET SDK (for C# agents)
# Download from https://dot.net/download
dotnet --version  # Verify >= 8.0

# Power Platform CLI (for Copilot Studio management)
npm install -g @microsoft/powerplatform-cli

# Teams Toolkit CLI (alternative to VS Code extension)
npm install -g @microsoft/teamsfx-cli

# Docker (for containerized deployments and MCP servers)
# Install Docker Desktop or Docker Engine
docker --version
docker-compose --version
```

## Docker — Container Setup

Docker is essential for hosting MCP servers, custom APIs, and containerized agent deployments.

### Dockerfile for Python Agent
```dockerfile
FROM python:3.12-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY src/ ./src/

# Expose the agent's port
EXPOSE 3978

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:3978/health || exit 1

# Run the agent
CMD ["python", "src/app.py"]
```

### Dockerfile for MCP Server
```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY mcp_server.py .

# MCP servers typically use SSE on port 8080
EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1

CMD ["python", "mcp_server.py"]
```

### Docker Compose for Local Development
```yaml
# docker-compose.yml
version: '3.8'
services:
  agent:
    build: .
    ports:
      - "3978:3978"
    environment:
      - AZURE_OPENAI_ENDPOINT=${AZURE_OPENAI_ENDPOINT}
      - AZURE_OPENAI_KEY=${AZURE_OPENAI_KEY}
      - MCP_INVENTORY_URL=http://mcp-inventory:8080
    depends_on:
      - mcp-inventory
      - redis

  mcp-inventory:
    build:
      context: ./mcp-servers/inventory
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=${DATABASE_URL}

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  # Agents Playground for local testing
  playground:
    image: mcr.microsoft.com/agents-playground:latest
    ports:
      - "5000:5000"
    environment:
      - AGENT_URL=http://agent:3978
```

### Building and Running
```bash
# Build all services
docker-compose build

# Run everything
docker-compose up

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f agent

# Tear down
docker-compose down
```

## Python Environment Setup

### Virtual Environment (Recommended for Development)
```bash
# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows

# Install dependencies
pip install -r requirements.txt
```

### Requirements File for Agent + Semantic Kernel
```
# requirements.txt
microsoft-agents-core>=1.0.0
microsoft-agents-hosting-aiohttp>=1.0.0
semantic-kernel>=1.0.0
azure-identity>=1.15.0
aiohttp>=3.9.0
python-dotenv>=1.0.0
```

### Requirements File for Agent + LangChain
```
# requirements.txt
microsoft-agents-core>=1.0.0
microsoft-agents-hosting-aiohttp>=1.0.0
langchain>=0.3.0
langchain-openai>=0.3.0
langgraph>=0.2.0
azure-identity>=1.15.0
python-dotenv>=1.0.0
```

### Requirements File for MCP Server
```
# requirements.txt
fastmcp>=2.0.0
azure-identity>=1.15.0
uvicorn>=0.30.0
```

## Bicep — Infrastructure as Code

Bicep is Microsoft's domain-specific language for Azure resource provisioning.
Preferred over ARM templates for readability and maintainability.

### Bicep CLI Setup
```bash
# Bicep comes with Azure CLI
az bicep install
az bicep version

# Verify Bicep works
az bicep build --file main.bicep
```

### Common Bicep Patterns for Agent Solutions

#### Resource Naming Convention
```bicep
// Use consistent naming: {type}-{project}-{environment}
var resourcePrefix = '${projectName}-${environmentName}'

// Examples:
// rg-myagent-dev        (Resource Group)
// app-myagent-dev       (App Service)
// kv-myagent-dev        (Key Vault)
// bot-myagent-dev       (Bot Service)
// appi-myagent-dev      (App Insights)
// id-myagent-dev-agent  (Managed Identity)
```

#### Modular Bicep Structure
```
infra/
├── main.bicep              # Entry point — orchestrates modules
├── main.parameters.json    # Environment-specific values
└── modules/
    ├── app-service.bicep   # Web app for hosting agent
    ├── bot-service.bicep   # Azure Bot Service registration
    ├── keyvault.bicep      # Key Vault for secrets
    ├── identity.bicep      # Managed Identity
    ├── monitoring.bicep    # App Insights + Log Analytics
    ├── openai.bicep        # Azure OpenAI service
    └── container-app.bicep # Container Apps (for MCP servers)
```

#### Main Bicep Entry Point
```bicep
// main.bicep
targetScope = 'resourceGroup'

@description('Environment name')
param environmentName string

@description('Project name')
param projectName string

@description('Azure region')
param location string = resourceGroup().location

// Tags applied to all resources
var tags = {
  environment: environmentName
  project: projectName
  'managed-by': 'platform-builder'
  'created-at': utcNow('yyyy-MM-dd')
}

// Modules
module identity 'modules/identity.bicep' = {
  name: 'identity'
  params: { resourcePrefix: '${projectName}-${environmentName}', location: location, tags: tags }
}

module keyvault 'modules/keyvault.bicep' = {
  name: 'keyvault'
  params: { resourcePrefix: '${projectName}-${environmentName}', location: location, tags: tags }
}

module monitoring 'modules/monitoring.bicep' = {
  name: 'monitoring'
  params: { resourcePrefix: '${projectName}-${environmentName}', location: location, tags: tags }
}

module appService 'modules/app-service.bicep' = {
  name: 'app-service'
  params: {
    resourcePrefix: '${projectName}-${environmentName}'
    location: location
    tags: tags
    managedIdentityId: identity.outputs.managedIdentityId
    appInsightsConnectionString: monitoring.outputs.appInsightsConnectionString
  }
}

module botService 'modules/bot-service.bicep' = {
  name: 'bot-service'
  params: {
    resourcePrefix: '${projectName}-${environmentName}'
    msAppId: identity.outputs.managedIdentityClientId
    botEndpoint: 'https://${appService.outputs.hostname}/api/messages'
    tags: tags
  }
}
```

### Deploying with Bicep
```bash
# Create resource group
az group create --name rg-myagent-dev --location eastus2

# Deploy infrastructure
az deployment group create \
  --resource-group rg-myagent-dev \
  --template-file infra/main.bicep \
  --parameters environmentName=dev projectName=myagent

# View deployment outputs
az deployment group show \
  --resource-group rg-myagent-dev \
  --name main \
  --query properties.outputs
```

## Azure Hosting Options for MCP Servers

### Azure Container Apps (Recommended)
Serverless containers with auto-scaling. Best for MCP servers.

```bicep
// modules/container-app.bicep
resource containerApp 'Microsoft.App/containerApps@2024-03-01' = {
  name: 'ca-${resourcePrefix}-mcp'
  location: location
  properties: {
    managedEnvironmentId: containerEnv.id
    configuration: {
      ingress: {
        external: true
        targetPort: 8080
        transport: 'http'
      }
    }
    template: {
      containers: [
        {
          name: 'mcp-server'
          image: '${acrName}.azurecr.io/mcp-server:latest'
          resources: {
            cpu: json('0.5')
            memory: '1Gi'
          }
        }
      ]
      scale: {
        minReplicas: 1
        maxReplicas: 10
      }
    }
  }
}
```

### Azure App Service
Traditional web hosting. Good for agents and simple MCP servers.

### Azure Functions
Event-driven, pay-per-execution. Good for lightweight tools.

### Azure Kubernetes Service (AKS)
Full Kubernetes. For complex multi-agent, multi-MCP-server architectures.

## Cost Considerations

### Development / Sandbox
- Azure OpenAI: ~$0.01-0.03 per 1K tokens (model dependent)
- App Service (B1): ~$13/month
- Container Apps: ~$0 for low traffic (consumption plan)
- Key Vault: ~$0.03 per 10K operations
- Bot Service: Free tier available

### Production
- Azure OpenAI: Volume dependent, budget $100-1000+/month
- App Service (P1v3): ~$100/month
- Container Apps: ~$50-200/month (traffic dependent)
- Application Insights: ~$2.30 per GB ingested
- Agent 365: $15/user/month (if applicable)

### Cost Optimization Tips
- Use consumption-based services where possible (Container Apps, Functions)
- Right-size compute — start small, scale as needed
- Use reserved instances for predictable workloads
- Monitor with Cost Management and set budgets/alerts
- Use Azure OpenAI provisioned throughput for high-volume scenarios
