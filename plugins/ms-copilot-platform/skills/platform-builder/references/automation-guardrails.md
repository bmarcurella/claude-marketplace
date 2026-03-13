# Automation Guardrails Framework

Safety rules and guardrails for the platform-builder's automated execution mode.

## Guardrail Tiers

### Tier 1: Non-Negotiable (Always Enforced)

These guardrails cannot be overridden:

1. **No secrets in plain text** — All secrets, keys, and connection strings must be stored in
   Azure Key Vault. Never write secrets to files, environment variables, or source code.

2. **No Owner/Global Admin auto-assignment** — Never automatically assign Owner, Global Admin,
   or Privileged Role Administrator. Always require manual elevation.

3. **Always use specified subscription** — Never create resources outside the user's
   specified Azure subscription and resource group.

4. **Always include cleanup** — Every provisioning step must have a corresponding teardown
   command documented and available.

5. **Cost gate for production** — Always show estimated monthly cost before provisioning
   any Azure resource to a production environment.

6. **No destructive operations without confirmation** — Never delete, overwrite, or modify
   existing production resources without explicit user confirmation.

7. **Audit trail** — Log every automated action with timestamp, resource, and operation type.

### Tier 2: Default-On (Can Be Relaxed for Dev/Sandbox)

These guardrails are on by default but can be relaxed for development environments:

1. **Approval before each phase** — Pause and show what will be created before executing.
   - Dev/sandbox: Can enable auto-approve for the full build
   - Production: Always requires phase-by-phase approval

2. **Cost threshold alerts** — Alert if estimated monthly cost exceeds $100.
   - Configurable threshold
   - Production: Alert at $50

3. **Resource naming validation** — Enforce naming conventions.
   - Dev: Warn on non-compliance
   - Production: Block on non-compliance

4. **Tag enforcement** — All Azure resources must be tagged.
   - Required tags: environment, project, owner, cost-center
   - Dev: Warn on missing tags
   - Production: Block on missing tags

### Tier 3: Configurable (User-Defined)

These guardrails are fully user-configurable:

1. **Allowed Azure regions** — Restrict to specific regions
2. **Allowed resource types** — Restrict to specific Azure resource types
3. **Budget cap** — Hard stop if monthly cost estimate exceeds threshold
4. **Required reviewers** — Specific users who must approve certain resource types
5. **Custom naming conventions** — Organization-specific naming patterns
6. **Allowed model deployments** — Restrict which AI models can be deployed

## Execution Modes

### Mode A: Guided Manual (Default)

```
For each build step:
1. Show what will be created and why
2. Show the exact command/action
3. Show expected output
4. Wait for user to execute manually or confirm auto-execution
5. Verify success
6. Proceed to next step
```

### Mode B: Automated with Checkpoints

```
For each build phase:
1. Show phase summary (what will be created, why, cost)
2. Wait for phase approval
3. Execute all steps in the phase automatically
4. Show results and verify
5. Proceed to next phase
```

### Mode C: Full Auto (Dev/Sandbox Only)

```
1. Show complete build plan with total cost estimate
2. Wait for one-time approval
3. Execute entire build automatically
4. Show final results and verification
```

**Mode C is only available when:**
- Target environment is explicitly tagged as dev/sandbox
- Total estimated cost is below the configured threshold
- No production data sources are being connected
- User has explicitly opted in to full auto mode

## Pre-Execution Checklist

Before any automated execution begins:

- [ ] Architecture design has been reviewed and approved
- [ ] Build Readiness Report shows no RED items
- [ ] User has confirmed available licenses and permissions
- [ ] Target subscription and resource group are specified
- [ ] Cost estimate has been acknowledged
- [ ] Execution mode has been selected
- [ ] Guardrail configuration has been reviewed

## Rollback Procedures

Every automated action generates a rollback entry:

```json
{
  "action": "create-resource-group",
  "resource": "rg-agent-solution-dev",
  "timestamp": "2026-03-12T10:30:00Z",
  "rollback_command": "az group delete --name rg-agent-solution-dev --yes",
  "dependencies": []
}
```

Rollback is executed in reverse order, respecting dependencies.

## Error Handling

When an automated step fails:

1. **Stop immediately** — Do not proceed to dependent steps
2. **Show the error** — Full error message and context
3. **Suggest fix** — Based on common error patterns
4. **Offer options:**
   - Retry the failed step
   - Skip (if not critical) and continue
   - Rollback all changes and start over
   - Switch to manual mode for this step

## Audit Log Format

```
[2026-03-12T10:30:00Z] [CREATE] Resource Group: rg-agent-solution-dev
  Location: eastus
  Tags: env=dev, project=hr-agent, owner=brandon
  Status: SUCCESS
  Cost Impact: $0/month

[2026-03-12T10:30:15Z] [CREATE] Azure AI Foundry Workspace: ai-hr-agent-dev
  Resource Group: rg-agent-solution-dev
  SKU: S0
  Status: SUCCESS
  Cost Impact: ~$45/month
```
