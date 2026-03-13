# Agent Governance Policy Templates

Starter policy templates for enterprise agent governance using Agent 365 and the
Microsoft Frontier Agent Platform control system.

## Policy 1: Agent Registration Policy

**Purpose:** Ensure all agents are registered and discoverable.

### Requirements
- All agents MUST be registered with Agent 365 before production deployment
- Registration must include: name, description, owner, team, data classification
- Agents must declare all data sources and permissions they require
- Third-party agents must pass security review before registration

### Exceptions
- Personal prototype agents in development environments (max 30-day exemption)
- Agents in Copilot Studio test canvas (not yet published)

## Policy 2: Agent Access Control Policy

**Purpose:** Ensure least-privilege access for all agents.

### Requirements
- Agents MUST use managed identities for service-to-service authentication
- Application permissions must be scoped to the minimum required
- User-delegated permissions preferred over application permissions where feasible
- Conditional access policies must apply to agent authentication flows
- No shared service accounts for agent access
- Agent credentials must be stored in Azure Key Vault

### Review Cadence
- Quarterly review of agent permissions
- Annual recertification of all agent access

## Policy 3: Data Handling Policy

**Purpose:** Protect organizational data accessed by agents.

### Data Classification
| Classification | Handling Rule |
|---------------|--------------|
| Public | Agent can access and share freely |
| Internal | Agent can access; must not share externally |
| Confidential | Agent can access with user consent; DLP policies apply |
| Highly Confidential | Agent must NOT access unless specifically approved |

### Requirements
- Agents must respect Microsoft Purview sensitivity labels
- DLP policies must be configured for all agents handling confidential data
- Agent responses must not leak data from higher classification to lower
- Audit logging required for all data access operations

## Policy 4: Agent Lifecycle Policy

**Purpose:** Manage agents from creation through retirement.

### Lifecycle Stages
1. **Development** — Building and testing in dev environment
2. **Review** — Security and governance review
3. **Staging** — Pre-production testing
4. **Production** — Active deployment
5. **Maintenance** — Ongoing updates and monitoring
6. **Retirement** — Decommissioning

### Requirements
- All agents must go through review before production
- Production agents must have an assigned owner and backup owner
- Agents unused for 90 days must be reviewed for retirement
- Retirement must include data cleanup and access revocation

## Policy 5: Monitoring and Incident Policy

**Purpose:** Ensure operational visibility and incident response.

### Monitoring Requirements
- All production agents must be instrumented with Copilot Analytics
- Error rate alerts: Trigger at >5% error rate
- Latency alerts: Trigger at >10s p95 response time
- Usage anomaly alerts: >3x normal volume or >50% drop

### Incident Response
- Agent-related incidents classified under existing IT incident process
- Agent owner is the first responder for agent-specific issues
- Security incidents (data breach, prompt injection) escalate to security team
- Ability to disable any agent within 15 minutes of incident declaration

## Policy 6: Third-Party Agent Policy

**Purpose:** Govern agents from external vendors.

### Requirements
- Third-party agents must be listed in the approved Agent Store catalog
- Vendor must provide: security assessment, data handling agreement, SLA
- All data access must go through organization's Entra ID (no direct vendor access)
- Quarterly vendor review and recertification
- Ability to revoke access within 1 hour

## Policy 7: Responsible AI Policy

**Purpose:** Ensure agents operate within responsible AI principles.

### Requirements
- Content safety filters enabled for all agents (Azure AI Content Safety)
- Agents must not generate harmful, biased, or misleading content
- User feedback mechanism required (thumbs up/down, report issues)
- Regular evaluation of agent outputs for quality and safety
- Prompt injection defenses enabled
- Clear disclosure that user is interacting with an AI agent

## Implementation Checklist

- [ ] Policies approved by IT governance board
- [ ] Agent 365 configured to enforce policies
- [ ] Entra ID conditional access policies created
- [ ] Purview DLP policies configured
- [ ] Copilot Analytics dashboards deployed
- [ ] Incident response runbook created
- [ ] Agent owner training completed
- [ ] Third-party vendor assessment process established
- [ ] Responsible AI review process in place
