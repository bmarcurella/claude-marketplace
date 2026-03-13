# Agent Review and Improvement Checklist

Use this checklist when reviewing an existing agent or evaluating agent quality after building.

## Instructions Quality
- [ ] Instructions are clear and specific
- [ ] Scope is well-defined (agent knows what it should NOT do)
- [ ] Tone and personality are appropriate for the audience
- [ ] Edge cases are addressed (what to say when confused, off-topic, etc.)
- [ ] Instructions reference specific knowledge sources where appropriate

## Knowledge Sources
- [ ] Sources are relevant and well-organized
- [ ] No stale or outdated content included
- [ ] Content volume is appropriate (not too much, not too little)
- [ ] Sources are properly scoped (no unnecessary access)
- [ ] Grounding quality is tested with real user questions

## Actions and Tools
- [ ] Each tool has clear description and parameters
- [ ] Error handling is implemented (what happens when API fails?)
- [ ] Authentication is properly configured
- [ ] Rate limits and timeouts are considered
- [ ] Tools follow least-privilege access

## Security and Governance
- [ ] Registered with Agent 365 (enterprise deployments)
- [ ] Entra app registration uses appropriate permissions
- [ ] Data access follows least-privilege principle
- [ ] Content moderation / safety filters enabled
- [ ] No secrets hardcoded in configuration
- [ ] Audit logging enabled

## User Experience
- [ ] Conversation starters are helpful and representative
- [ ] Agent responds appropriately to greetings and off-topic messages
- [ ] Response format is consistent and readable
- [ ] Adaptive Cards render correctly (if used)
- [ ] Error messages are user-friendly

## Agentic Maturity Assessment
- [ ] Could this agent use more tools to be more helpful?
- [ ] Could this agent handle multi-step workflows?
- [ ] Should this agent coordinate with other agents (A2A)?
- [ ] Could this agent be proactive / autonomous?
- [ ] Is the orchestration framework appropriate for complexity?

## Performance
- [ ] Response time is acceptable (<5s for most queries)
- [ ] Token usage is efficient (not over-retrieving knowledge)
- [ ] Monitoring is set up (Copilot Analytics or App Insights)
- [ ] Error rates are tracked
