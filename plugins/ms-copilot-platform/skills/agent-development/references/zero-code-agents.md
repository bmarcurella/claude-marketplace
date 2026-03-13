# Zero-Code Agents Guide

Build agents without writing any code using SharePoint Agents and Agent Builder.

## SharePoint Agent

### What It Is
An instant Q&A agent over a SharePoint document library. Takes 2 minutes to create.

### How to Create
1. Navigate to any SharePoint document library
2. Click the Copilot icon in the library toolbar
3. Select "Create Agent"
4. The agent automatically indexes all documents in the library
5. Users can immediately ask questions about the documents

### Capabilities
- Answers questions from documents in the library
- Cites source documents in responses
- Accessible via M365 Copilot chat
- Automatically updates when documents change
- No configuration needed

### Limitations
- Single library only (cannot span multiple libraries)
- No custom instructions
- No actions or workflow automation
- No connectors to external data
- Response quality depends on document quality

### Best For
Quick departmental knowledge bases, project documentation Q&A, policy lookups.

## Agent Builder

### What It Is
A slightly more configurable version — create agents with custom instructions,
multiple knowledge sources, and conversation starters. Still zero code.

### How to Create
1. Open M365 Copilot Chat (copilot.microsoft.com)
2. Click "Create" → "Agent"
3. Configure:
   - **Name and description** — What the agent does
   - **Instructions** — How the agent should behave (natural language)
   - **Knowledge sources** — SharePoint sites, web URLs, files
   - **Conversation starters** — Suggested first questions

### Capabilities
- Custom instructions (system prompt in natural language)
- Multiple knowledge sources (SharePoint sites + web URLs)
- Conversation starters for user guidance
- Accessible via M365 Copilot

### Limitations
- No connectors to external systems
- No workflow automation or triggers
- No API integration
- No Adaptive Cards
- Limited customization compared to Copilot Studio

### Best For
Team-specific assistants, onboarding helpers, FAQ bots with custom personality.

## When to Graduate Up

Move to Copilot Studio when:
- Need connectors to external systems (CRM, ITSM, etc.)
- Need workflow automation (approvals, notifications)
- Need autonomous triggers (proactive actions)
- Need multi-channel deployment beyond Copilot

Move to Declarative Agent when:
- Need API integrations via OpenAPI
- Need source control and CI/CD
- Need Adaptive Cards for rich responses
- Need more precise control over behavior
