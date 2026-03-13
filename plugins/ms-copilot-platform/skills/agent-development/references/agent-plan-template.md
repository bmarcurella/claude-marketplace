# Agent Design Plan Template

Use this template when producing an Agent Design Plan after the user selects one of the
3 presented options. Every field should be filled in with specifics — no placeholders.

## Full Template

```
╔══════════════════════════════════════════════════════════════╗
║                    AGENT DESIGN PLAN                        ║
║                    [Agent Name]                             ║
╚══════════════════════════════════════════════════════════════╝

━━━ 1. OVERVIEW ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Agent Name:     [Name]
Agent Type:     [SharePoint Agent / Agent Builder / Copilot Studio /
                 Declarative Agent / Custom Engine Agent]
Purpose:        [What this agent does, in 2-3 sentences]
Primary Users:  [Who uses this agent]
Channels:       [Where the agent is accessible]
Success Criteria:
  - [Measurable outcome 1]
  - [Measurable outcome 2]
  - [Measurable outcome 3]

━━━ 2. ARCHITECTURE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Mermaid diagram showing components and data flow]

Components:
  ┌─────────────────┬────────────────────┬───────────────┐
  │ Component       │ Purpose            │ Technology    │
  ├─────────────────┼────────────────────┼───────────────┤
  │ [Component 1]   │ [What it does]     │ [Tech choice] │
  │ [Component 2]   │ [What it does]     │ [Tech choice] │
  │ [Component 3]   │ [What it does]     │ [Tech choice] │
  └─────────────────┴────────────────────┴───────────────┘

Data Flow:
  1. User sends message via [channel]
  2. Message routes to [component]
  3. [Component] processes with [AI service]
  4. [Component] calls [tools/APIs] as needed
  5. Response returns to user via [channel]

━━━ 3. REQUIREMENTS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Prerequisites:
  Licenses:
    □ [License 1] — [why needed] — [cost]
    □ [License 2] — [why needed] — [cost]

  Azure Subscription:
    □ [Subscription type] — [why needed]
    □ [Quota requirements if any]

  Permissions:
    □ [Permission 1] — [who needs to grant it]
    □ [Permission 2] — [who needs to grant it]

Infrastructure:
  Azure Resources:
    □ [Resource 1] — [SKU/tier] — [est. monthly cost]
    □ [Resource 2] — [SKU/tier] — [est. monthly cost]

  Container Requirements:
    □ Docker: [Yes/No] — [what it hosts]
    □ Container Registry: [Yes/No]

  Runtime Environments:
    □ [Python 3.12 / Node.js 20 / .NET 8] — [for what component]

Development Tools:
    □ VS Code with [extensions list]
    □ [CLI tools list]
    □ [SDK/package list]

Data Access:
  Connectors / MCP Servers:
    □ [Connector 1] — [data source] — [permissions needed]
    □ [Connector 2] — [data source] — [permissions needed]

  APIs:
    □ [API 1] — [endpoint] — [auth method]
    □ [API 2] — [endpoint] — [auth method]

━━━ 4. BUILD SEQUENCE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Phase 1: Foundation                        [Est. time: X hours]
  ┌──────────────────────────────────────────────────────┐
  │ Step 1.1: [Action]                                   │
  │   Why: [Reason this must come first]                 │
  │   How: [Brief description or command]                │
  │   Verify: [How to confirm success]                   │
  │                                                      │
  │ Step 1.2: [Action]                                   │
  │   Why: [Reason]                                      │
  │   How: [Description]                                 │
  │   Verify: [Confirmation method]                      │
  └──────────────────────────────────────────────────────┘

Phase 2: Core Agent                        [Est. time: X hours]
  ┌──────────────────────────────────────────────────────┐
  │ Step 2.1: [Scaffold project]                         │
  │ Step 2.2: [Configure AI service]                     │
  │ Step 2.3: [Implement core agent logic]               │
  │ Step 2.4: [Add custom instructions]                  │
  │ Step 2.5: [Test locally]                             │
  └──────────────────────────────────────────────────────┘

Phase 3: Integrations                      [Est. time: X hours]
  ┌──────────────────────────────────────────────────────┐
  │ Step 3.1: [Connect knowledge sources]                │
  │ Step 3.2: [Configure tools/actions]                  │
  │ Step 3.3: [Set up MCP server connections]            │
  │ Step 3.4: [Configure connectors]                     │
  │ Step 3.5: [Test integrations]                        │
  └──────────────────────────────────────────────────────┘

Phase 4: Polish                            [Est. time: X hours]
  ┌──────────────────────────────────────────────────────┐
  │ Step 4.1: [Conversation starters / UX]               │
  │ Step 4.2: [Error handling and fallbacks]             │
  │ Step 4.3: [Adaptive Cards (if applicable)]           │
  │ Step 4.4: [End-to-end testing]                       │
  └──────────────────────────────────────────────────────┘

Phase 5: Governance & Deployment           [Est. time: X hours]
  ┌──────────────────────────────────────────────────────┐
  │ Step 5.1: [Entra app registration]                   │
  │ Step 5.2: [Agent 365 registration]                   │
  │ Step 5.3: [Monitoring and analytics setup]           │
  │ Step 5.4: [Deploy to target environment]             │
  │ Step 5.5: [Publish to channels]                      │
  └──────────────────────────────────────────────────────┘

━━━ 5. ESTIMATED EFFORT & COST ━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Build Time:
  Total: [X hours/days]
  Phase 1: [X hours]
  Phase 2: [X hours]
  Phase 3: [X hours]
  Phase 4: [X hours]
  Phase 5: [X hours]

Monthly Ongoing Costs:
  ┌─────────────────────┬──────────────┐
  │ Item                │ Est. Cost    │
  ├─────────────────────┼──────────────┤
  │ [Azure hosting]     │ $XXX/month   │
  │ [AI model usage]    │ $XXX/month   │
  │ [Licenses]          │ $XXX/month   │
  │ [Connectors]        │ $XXX/month   │
  ├─────────────────────┼──────────────┤
  │ TOTAL               │ $XXX/month   │
  └─────────────────────┴──────────────┘

━━━ 6. EXECUTION MODE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Choose how to proceed:

  □ GUIDED WALKTHROUGH
    Walk through each step with explanations.
    Learn how and why at each stage.
    Estimated time: [X hours with learning]

  □ AUTONOMOUS BUILD
    Generate all artifacts and execute automatically.
    Produces complete project ready to deploy.
    Estimated time: [X minutes for generation]

  □ HYBRID
    Autonomous build for infrastructure and scaffolding,
    guided walkthrough for agent logic and customization.
    Estimated time: [X hours]
```

## Plan Presentation Guidelines

When presenting the plan:

1. Use the architecture diagram to show how components connect
2. Highlight any decisions the user needs to make
3. Call out prerequisites that may take time (admin approvals, license procurement)
4. Be specific about costs — don't just say "varies"
5. Recommend the execution mode based on user's stated skill level
6. Offer to adjust the plan before proceeding

## After Plan Approval

Once the user approves:

1. Confirm the execution mode
2. Check prerequisites one more time
3. Begin Phase 1
4. Update progress after each phase
5. Verify success criteria at the end
