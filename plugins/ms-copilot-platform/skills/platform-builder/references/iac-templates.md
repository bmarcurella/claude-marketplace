# Infrastructure as Code Templates

Bicep and ARM template patterns for provisioning Azure resources for agent solutions.

## Bicep Template: Basic Agent Infrastructure

```bicep
// agent-foundation.bicep
// Provisions the foundational Azure resources for an agent solution

@description('Environment name (dev, staging, prod)')
param environmentName string = 'dev'

@description('Azure region for resources')
param location string = resourceGroup().location

@description('Project name for resource naming')
param projectName string

@description('Cost center tag')
param costCenter string = ''

// Variables
var resourcePrefix = '${projectName}-${environmentName}'
var tags = {
  environment: environmentName
  project: projectName
  'cost-center': costCenter
  'managed-by': 'platform-builder'
}

// Key Vault for secrets
resource keyVault 'Microsoft.KeyVault/vaults@2023-07-01' = {
  name: 'kv-${resourcePrefix}'
  location: location
  tags: tags
  properties: {
    tenantId: subscription().tenantId
    sku: {
      family: 'A'
      name: 'standard'
    }
    enableRbacAuthorization: true
    enableSoftDelete: true
    softDeleteRetentionInDays: 30
  }
}

// Managed Identity for agent
resource managedIdentity 'Microsoft.ManagedIdentity/userAssignedIdentities@2023-01-31' = {
  name: 'id-${resourcePrefix}-agent'
  location: location
  tags: tags
}

// Application Insights for monitoring
resource appInsights 'Microsoft.Insights/components@2020-02-02' = {
  name: 'appi-${resourcePrefix}'
  location: location
  tags: tags
  kind: 'web'
  properties: {
    Application_Type: 'web'
  }
}

// Outputs
output keyVaultName string = keyVault.name
output managedIdentityId string = managedIdentity.id
output managedIdentityClientId string = managedIdentity.properties.clientId
output appInsightsConnectionString string = appInsights.properties.ConnectionString
```

## Bicep Template: Azure AI Foundry Workspace

```bicep
// foundry-workspace.bicep
// Provisions Azure AI Foundry workspace for agent hosting

@description('Resource prefix')
param resourcePrefix string

@description('Location')
param location string = resourceGroup().location

@description('Tags')
param tags object = {}

@description('Key Vault name for secrets')
param keyVaultName string

// AI Foundry Hub
resource aiHub 'Microsoft.MachineLearningServices/workspaces@2024-10-01' = {
  name: 'aih-${resourcePrefix}'
  location: location
  tags: tags
  kind: 'Hub'
  identity: {
    type: 'SystemAssigned'
  }
  properties: {
    friendlyName: '${resourcePrefix} AI Hub'
    keyVault: resourceId('Microsoft.KeyVault/vaults', keyVaultName)
  }
}

// AI Foundry Project
resource aiProject 'Microsoft.MachineLearningServices/workspaces@2024-10-01' = {
  name: 'aip-${resourcePrefix}'
  location: location
  tags: tags
  kind: 'Project'
  identity: {
    type: 'SystemAssigned'
  }
  properties: {
    friendlyName: '${resourcePrefix} Agent Project'
    hubResourceId: aiHub.id
  }
}

output aiHubName string = aiHub.name
output aiProjectName string = aiProject.name
```

## Bicep Template: Bot Service Registration

```bicep
// bot-service.bicep
// Registers a bot/agent with Azure Bot Service for multi-channel deployment

@description('Resource prefix')
param resourcePrefix string

@description('Location')
param location string = 'global'

@description('Tags')
param tags object = {}

@description('Microsoft App ID for the bot')
param msAppId string

@description('Bot display name')
param botDisplayName string

resource botService 'Microsoft.BotService/botServices@2022-09-15' = {
  name: 'bot-${resourcePrefix}'
  location: location
  tags: tags
  kind: 'azurebot'
  sku: {
    name: 'S1'
  }
  properties: {
    displayName: botDisplayName
    msaAppId: msAppId
    endpoint: 'https://${resourcePrefix}.azurewebsites.net/api/messages'
    msaAppType: 'UserAssignedMSI'
  }
}

// Teams channel
resource teamsChannel 'Microsoft.BotService/botServices/channels@2022-09-15' = {
  parent: botService
  name: 'MsTeamsChannel'
  location: location
  properties: {
    channelName: 'MsTeamsChannel'
  }
}

output botServiceName string = botService.name
```

## PowerShell: Entra App Registration

```powershell
# register-agent-app.ps1
# Creates Entra ID app registration for an agent

param(
    [Parameter(Mandatory)]
    [string]$AppName,

    [Parameter(Mandatory)]
    [string]$EnvironmentName,

    [string[]]$RequiredGraphPermissions = @(
        "User.Read",
        "Mail.Read",
        "Calendars.Read"
    )
)

$displayName = "$AppName-$EnvironmentName"

# Create app registration
$app = az ad app create `
    --display-name $displayName `
    --sign-in-audience "AzureADMyOrg" `
    --output json | ConvertFrom-Json

Write-Host "Created app registration: $($app.appId)"

# Add required Graph permissions
foreach ($permission in $RequiredGraphPermissions) {
    # Note: In practice, look up the permission ID from Microsoft Graph
    Write-Host "Adding permission: $permission"
}

# Create service principal
az ad sp create --id $app.appId

Write-Host "App Registration Complete"
Write-Host "  App ID: $($app.appId)"
Write-Host "  Object ID: $($app.id)"
Write-Host ""
Write-Host "NEXT STEPS:"
Write-Host "  1. Grant admin consent for the requested permissions"
Write-Host "  2. Store the app ID in Key Vault"
Write-Host "  3. Configure the agent to use this app registration"
```

## Cleanup Script Template

```powershell
# cleanup-agent-resources.ps1
# Removes all resources created by the platform builder

param(
    [Parameter(Mandatory)]
    [string]$ResourceGroupName,

    [switch]$Force
)

if (-not $Force) {
    Write-Warning "This will delete ALL resources in resource group: $ResourceGroupName"
    $confirm = Read-Host "Type 'DELETE' to confirm"
    if ($confirm -ne 'DELETE') {
        Write-Host "Cancelled."
        exit 0
    }
}

# Remove app registrations first (not in resource group)
Write-Host "Checking for app registrations to remove..."
# [App registration cleanup logic]

# Delete resource group (deletes all contained resources)
Write-Host "Deleting resource group: $ResourceGroupName"
az group delete --name $ResourceGroupName --yes --no-wait

Write-Host "Cleanup initiated. Resource group deletion is in progress."
Write-Host "Use 'az group show -n $ResourceGroupName' to check status."
```

## Usage Notes

- Always parameterize templates — never hardcode values
- Use consistent naming conventions with environment prefix
- Tag every resource for cost tracking and governance
- Include outputs for downstream dependencies
- Store all templates in source control
- Use Bicep parameter files for environment-specific values
