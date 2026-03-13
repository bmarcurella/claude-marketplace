# Solution Architecture Requirements Template

Use this template to gather requirements before designing an agent solution architecture.

## 1. Business Context

### Problem Statement
- What business problem does this agent solve?
- What manual process does it replace or augment?
- What's the expected business value?

### Users / Personas
- Who will interact with the agent?
- How many users (approximate)?
- Technical skill level of users?
- Are users internal, external, or both?

## 2. Functional Requirements

### Core Capabilities
- What should the agent DO? (List specific capabilities)
- What should the agent NOT do? (Scope boundaries)
- Should the agent be reactive (respond to queries) or proactive (initiate actions)?
- Should the agent coordinate with other agents?

### Data & Knowledge
- What data sources does the agent need?
  - [ ] SharePoint sites/libraries
  - [ ] Microsoft Graph data (mail, calendar, files, people)
  - [ ] Dynamics 365 data
  - [ ] External APIs
  - [ ] Databases (SQL, Cosmos, etc.)
  - [ ] Third-party systems (Salesforce, SAP, ServiceNow, etc.)
  - [ ] Custom files/documents
  - [ ] Web content
- How often does the data change?
- Is the data sensitive or classified?

### Actions & Integrations
- What actions should the agent perform?
  - [ ] Read/retrieve information
  - [ ] Create/update records
  - [ ] Send messages/emails
  - [ ] Trigger workflows
  - [ ] Generate documents
  - [ ] Schedule meetings
  - [ ] Approve/reject requests
- What systems does it need to integrate with?

### Channels & Surfaces
- Where should the agent be available?
  - [ ] Microsoft 365 Copilot
  - [ ] Microsoft Teams
  - [ ] Outlook
  - [ ] SharePoint
  - [ ] Web (custom website)
  - [ ] Mobile app
  - [ ] Email
  - [ ] Other messaging platforms

## 3. Non-Functional Requirements

### Performance
- Expected response time (seconds)?
- Expected concurrent users?
- Expected messages per hour/day?

### Security & Compliance
- Authentication requirements (SSO, MFA)?
- Data residency requirements?
- Regulatory compliance (GDPR, HIPAA, SOC 2, etc.)?
- Data classification of handled information?
- DLP requirements?

### Availability
- Required uptime SLA?
- Disaster recovery requirements?
- Maintenance window constraints?

### Scalability
- Growth expectations (users, data volume)?
- Multi-region requirements?
- Multi-language requirements?

## 4. Technical Constraints

### Existing Infrastructure
- Current Microsoft 365 licensing tier?
- Current Azure subscription?
- Existing Power Platform environments?
- Existing identity infrastructure (Entra ID, on-prem AD)?

### Development Team
- Team size and skill composition?
- Preferred programming languages?
- CI/CD tooling in use?
- Source control platform?

### Budget
- Estimated budget range?
- Cost model preference (consumption vs. committed)?

## 5. Timeline & Priorities

### Phasing
- Phase 1 MVP scope?
- Phase 2 enhancements?
- Phase 3 long-term vision?

### Timeline
- Target date for Phase 1?
- Key milestones?
- Dependencies on other projects?

## 6. Success Criteria

- How will success be measured?
- Key metrics to track?
- Minimum viable performance thresholds?
