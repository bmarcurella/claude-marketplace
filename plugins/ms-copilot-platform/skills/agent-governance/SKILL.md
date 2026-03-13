---
name: agent-governance
description: >
  This skill should be used when the user asks about "Agent 365", "agent governance",
  "agent security", "Entra for agents", "Purview for agents", "Defender for agents",
  "Copilot Analytics", "agent compliance", "agent monitoring", "agent operations",
  "agent registry", "agent quotas", "rate limiting for agents", "TPM PTU",
  "responsible AI for agents", "guardrails for agents", "agent lifecycle management",
  "agent identity", "agent access control", or any question about managing, governing,
  securing, monitoring, or operating agents at enterprise scale. Covers the full Agent
  Control System including Agent 365, Copilot Analytics, and Operations.
---

# Agent Governance & Control System

This skill covers the right side of the Frontier Agent Platform — the Agent Control System
that provides enterprise governance, security, compliance, monitoring, and operations for
all agents across the organization.

## Agent 365 — The Control Plane

Agent 365 is the unified governance and management layer for all agents. Components:

### Entra (Agent Identity)
- Agent identity and authentication via Microsoft Entra ID
- Managed identities for agent-to-service authentication
- Conditional access policies scoped to agents
- Role-based access control (RBAC) for agent permissions
- OAuth 2.0 flows for user-delegated and app-only access

### Purview (Security & Observability)
- Data governance for agent interactions
- Sensitivity labeling on agent-processed content
- Data loss prevention (DLP) policies for agent outputs
- Audit logging of all agent actions
- Information barriers between agent scopes

### Defender (Threat Protection)
- Threat detection for malicious agent behavior
- Prompt injection defense
- Data exfiltration monitoring
- Anomaly detection on agent activity patterns
- Security incident response for agent-related threats

### Admin (Registry, Tools, Usage Data)
- Central agent registry — discover all agents in the org
- Agent lifecycle management (create, publish, update, retire)
- Usage analytics and adoption metrics
- Configuration management and policy enforcement
- Projects and AI Gateway management

## Copilot Analytics

Monitoring and insights for agent performance:

- Agent usage metrics (sessions, messages, users, satisfaction)
- Performance dashboards (latency, error rates, throughput)
- User feedback analysis (thumbs up/down, explicit feedback)
- Knowledge source effectiveness (which sources get cited)
- Action success rates (which tools/APIs work well)

## Operations

Day-to-day operational management:

### Assets
- Agents, Models, and Tools inventory
- Version management and rollback capabilities
- Dependency tracking between agents and resources

### Compliance
- Policy enforcement engine
- Guardrail configuration and management
- Responsible AI compliance checks
- Regulatory alignment (GDPR, HIPAA, SOC 2)

### Quota
- TPM (Tokens Per Minute) management
- PTU (Provisioned Throughput Units) allocation
- Rate-limiting configuration
- Cost monitoring and budget controls

### All Agents View
The unified view of every agent in the organization:
- Copilot Studio agents
- Foundry agents
- Open AI agents
- Gemini agents (third-party)
- AWS Bedrock agents (third-party)
- Marketplace agents

## Governance Design Workflow

### Step 1: Define Agent Policies
Establish organizational policies for agent creation, data access, and behavior.
Read `references/governance-policies.md` for policy templates.

### Step 2: Configure Identity & Access
Set up Entra ID for agent authentication. Define RBAC roles.
Read `references/identity-access.md` for configuration patterns.

### Step 3: Enable Security Controls
Configure Purview, Defender, and DLP policies.
Read `references/security-controls.md` for security hardening.

### Step 4: Set Up Monitoring
Deploy Copilot Analytics dashboards and alerting.
Read `references/monitoring-setup.md` for dashboard templates.

### Step 5: Establish Operations
Define agent lifecycle processes, quota management, and incident response.

## Best Practices

1. **Register every agent** with Agent 365 — no shadow agents
2. **Least privilege** — agents should only access what they need
3. **Monitor from day one** — don't wait for problems to instrument
4. **Define guardrails before building** — responsible AI controls are easier to add early
5. **Plan for multi-vendor** — the platform supports agents from multiple providers
6. **Version and audit** — every change to an agent should be tracked

## Reference Files

- **`references/governance-policies.md`** — Policy templates for agent governance
- **`references/identity-access.md`** — Entra ID setup and RBAC patterns
- **`references/security-controls.md`** — Purview, Defender, and DLP configuration
- **`references/monitoring-setup.md`** — Copilot Analytics dashboards and alerting
