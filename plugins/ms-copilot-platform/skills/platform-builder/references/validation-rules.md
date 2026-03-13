# Design Validation Rules

Complete ruleset for validating an architecture design before the platform-builder begins
generating artifacts and provisioning infrastructure.

## Validation Categories

### Category 1: Completeness

Every design must specify:

| Required Element | Validation Rule | Severity |
|-----------------|----------------|----------|
| Agent type | At least one agent type selected | RED (must fix) |
| Hosting target | Hosting service specified for each agent | RED |
| Data sources | At least one data source identified | RED |
| Authentication method | Auth approach defined | RED |
| Target channels | At least one deployment channel | YELLOW (warn) |
| Governance plan | Agent 365 registration planned | YELLOW |
| Monitoring approach | Analytics/monitoring specified | YELLOW |
| Cost estimate | Budget or cost model identified | YELLOW |

### Category 2: Compatibility

Components must work together:

| Rule | Description | Severity |
|------|-----------|----------|
| Declarative + Copilot | Declarative agents require M365 Copilot as surface | RED |
| Copilot Studio + Power Platform | Copilot Studio needs Power Platform environment | RED |
| Custom Engine + Hosting | Custom engine needs Azure hosting (Foundry or self-hosted) | RED |
| MCP + Agent Type | MCP server access requires custom engine or Copilot Studio | YELLOW |
| A2A + Custom Engine | A2A protocol requires custom engine agents | RED |
| Fabric IQ + Fabric | Fabric IQ requires Microsoft Fabric workspace | RED |
| Computer Use + Windows 365 | Computer use agents require Windows 365 environment | RED |

### Category 3: Prerequisites

User must have:

| Prerequisite | Check Method | Severity |
|-------------|-------------|----------|
| M365 Copilot license | Ask user to confirm | RED if using Copilot surface |
| Copilot Studio license | Ask user to confirm | RED if using Copilot Studio |
| Azure subscription | Ask for subscription ID | RED if using Azure resources |
| Power Platform environment | Ask user to confirm | RED if using Power Platform |
| Global/Teams admin | Ask about admin access | YELLOW for publishing |
| Entra ID admin | Ask about admin access | YELLOW for app registrations |
| Fabric workspace | Ask user to confirm | RED if using Fabric |

### Category 4: Security

| Rule | Description | Severity |
|------|-----------|----------|
| No hardcoded secrets | All secrets must use Key Vault | RED |
| Least privilege | Permissions scoped to minimum required | YELLOW |
| Data classification | Sensitive data handling specified | YELLOW |
| DLP policies | Data loss prevention considered | YELLOW |
| Conditional access | MFA/CA policies for agent access | YELLOW |
| Audit logging | Agent actions auditable | YELLOW |

### Category 5: Cost

| Rule | Description | Severity |
|------|-----------|----------|
| Azure resources estimated | Cost for compute, storage, AI models | YELLOW |
| License requirements clear | All required licenses identified | YELLOW |
| Consumption model understood | Pay-per-use vs. committed pricing | YELLOW |
| Scale implications | Cost at expected scale documented | YELLOW |

## Build Readiness Report Format

```
╔══════════════════════════════════════╗
║     BUILD READINESS REPORT           ║
║     [Solution Name]                  ║
╠══════════════════════════════════════╣
║                                      ║
║  ● GREEN (Ready to Build)            ║
║    • [Item 1]                        ║
║    • [Item 2]                        ║
║                                      ║
║  ● YELLOW (Attention Needed)         ║
║    • [Item 1] - [Recommendation]     ║
║    • [Item 2] - [Recommendation]     ║
║                                      ║
║  ● RED (Must Resolve)                ║
║    • [Item 1] - [What's needed]      ║
║    • [Item 2] - [What's needed]      ║
║                                      ║
╠══════════════════════════════════════╣
║  Estimated Build Time: [X hours]     ║
║  Estimated Monthly Cost: [$X-$Y]     ║
║  Required Permissions: [List]        ║
╚══════════════════════════════════════╝
```

## Validation Execution Order

1. Run completeness checks first (catch missing elements early)
2. Run compatibility checks (catch impossible combinations)
3. Run prerequisite checks (catch environment blockers)
4. Run security checks (catch governance gaps)
5. Run cost checks (catch budget surprises)
6. Generate Build Readiness Report
7. Present to user for approval before proceeding
