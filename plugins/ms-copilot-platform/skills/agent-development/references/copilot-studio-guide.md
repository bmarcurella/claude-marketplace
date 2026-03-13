# Copilot Studio Development Guide

Comprehensive guide for building agents with Microsoft Copilot Studio — from basic setup
to advanced autonomous agents.

## What is Copilot Studio?

Copilot Studio is Microsoft's low-code/no-code platform for building AI agents. It provides:
- Visual topic and conversation designer
- 1000+ pre-built connectors (Power Platform connector ecosystem)
- Generative AI-powered topics (no manual dialog tree required)
- Knowledge source integration (SharePoint, web, Dataverse, files)
- Autonomous agent capabilities (event-driven triggers)
- Multi-channel publishing (Teams, web, Copilot, custom channels)
- Power Automate integration for workflow execution

## Two Editions

### Copilot Studio Lite
- Zero-code experience
- Natural language agent creation
- Basic knowledge sources
- Simple conversation flows
- Ideal for business users with no technical background

### Copilot Studio Full
- Full visual designer
- Advanced topics and entity extraction
- Power Automate flow integration
- Custom connectors
- Plugin actions
- Autonomous triggers
- Source control integration (export/import solutions)

## Getting Started

### Prerequisites
- Copilot Studio license (standalone or included in M365 plan)
- Power Platform environment
- Appropriate data access permissions

### Creating an Agent

1. Navigate to copilotstudio.microsoft.com
2. Select "Create" → "New agent"
3. Choose creation method:
   - **Describe it** — Natural language description, AI builds the agent
   - **From template** — Start from a pre-built template
   - **Blank** — Start from scratch with full control

### Configuring Knowledge Sources

Knowledge sources ground the agent's responses in organizational data:
- **SharePoint sites** — Point to specific site URLs
- **Web URLs** — Public websites for reference
- **Dataverse tables** — Structured business data
- **Files** — Upload documents directly
- **Graph connectors** — Enterprise search results

### Building Topics

Topics define conversation flows:

#### Generative Topics (Recommended)
Let the AI handle conversation routing and response generation based on:
- Agent instructions (system prompt)
- Knowledge sources
- Available actions

#### Manual Topics
For specific, deterministic flows:
- Trigger phrases
- Question nodes
- Condition branches
- Action nodes (Power Automate)
- Message nodes
- Redirect nodes

### Adding Actions

Actions let the agent DO things:
- **Power Automate flows** — Execute business logic, call APIs, update records
- **Plugin actions** — Pre-built API integrations
- **Custom connectors** — Connect to any REST API
- **Dataverse operations** — CRUD on business data

### Autonomous Triggers

Make agents proactive:
- **Schedule triggers** — Run at specific times
- **Event triggers** — React to system events
- **Dataverse triggers** — React to data changes
- **Email triggers** — Process incoming emails

### Publishing

Deploy the agent to channels:
- **Microsoft Teams** — Most common deployment
- **Microsoft 365 Copilot** — Surface in Copilot chat
- **Custom websites** — Embed via iframe or SDK
- **Mobile apps** — Via Direct Line API
- **Facebook, etc.** — Third-party messaging channels

## Advanced Capabilities

### Generative AI Orchestration
Configure how the AI handles conversations:
- System instructions (personality, scope, constraints)
- Knowledge source priority and grounding
- Fallback behavior
- Content moderation settings

### Multi-Turn Conversations
- Context retention across turns
- Slot filling for information gathering
- Disambiguation and clarification
- Topic switching and return

### Authentication
- User authentication via Entra ID
- Service-to-service authentication for connectors
- OAuth for third-party integrations

### Analytics
- Session analytics (volume, completion, satisfaction)
- Topic analytics (usage, escalation, abandonment)
- Knowledge source effectiveness
- Action success rates

## Best Practices

1. **Start with generative topics** — Let AI handle routing before building manual topics
2. **Write clear instructions** — The agent's system prompt is the most important config
3. **Scope knowledge sources** — Too much knowledge leads to poor grounding
4. **Test iteratively** — Use the test canvas after every change
5. **Monitor after publishing** — Analytics reveal what users actually ask vs. what you expected
6. **Version with solutions** — Export as Power Platform solutions for source control
7. **Use environment strategy** — Dev → Test → Production environment promotion

## Integration with Platform Builder

When the platform-builder skill automates Copilot Studio setup, it generates:
- Power Platform solution packages for import
- Agent configuration JSON
- Knowledge source connection details
- Power Automate flow definitions
- Publishing channel configurations

Always verify generated configurations by importing into a dev environment first.

## Fetching Latest Documentation

Use `microsoft_docs_search` with queries like:
- "Copilot Studio create agent"
- "Copilot Studio autonomous triggers"
- "Copilot Studio knowledge sources"
- "Copilot Studio Power Automate integration"
- "Copilot Studio publishing channels"

The Copilot Studio UI and capabilities change frequently — always verify against current docs.
