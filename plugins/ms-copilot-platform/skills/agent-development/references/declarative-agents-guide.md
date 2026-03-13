# Declarative Agents Guide

Declarative agents extend M365 Copilot with custom instructions, knowledge, and API actions
using JSON-based manifests. No server hosting required — Microsoft runs the agent.

## What Declarative Agents Are

A declarative agent is a JSON manifest that tells M365 Copilot:
- How to behave (custom instructions)
- What to know (knowledge sources — SharePoint, web, files, Graph connectors)
- What to do (API plugins via OpenAPI specs)

The agent runs inside Copilot — no backend hosting, no model selection, no infrastructure.

## When to Choose Declarative

Choose when: custom instructions + organizational knowledge + simple API actions are sufficient.
Skip when: custom AI model, complex orchestration, multi-channel, or autonomous triggers needed.

## Development with Agents Toolkit (VS Code)

### Setup
1. Install the Microsoft 365 Agents Toolkit extension in VS Code
2. Sign in with M365 developer account
3. Create New Agent → Declarative Agent

### Project Structure
```
my-declarative-agent/
├── appPackage/
│   ├── manifest.json            # M365 app manifest
│   ├── declarativeAgent.json    # Agent definition
│   ├── instructions.md          # Custom instructions (system prompt)
│   ├── outline.png
│   └── color.png
├── api-plugins/                 # Optional API integrations
│   ├── openapi.yml              # OpenAPI spec for APIs
│   └── ai-plugin.json           # Plugin metadata
└── README.md
```

### declarativeAgent.json
```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.4/schema.json",
  "version": "v1.4",
  "name": "Contoso HR Agent",
  "description": "Answers HR questions using company policies and benefits documentation",
  "instructions": "$[file('instructions.md')]",
  "capabilities": [
    {
      "name": "OneDriveAndSharePoint",
      "items_by_url": [
        { "url": "https://contoso.sharepoint.com/sites/HR/Policies" },
        { "url": "https://contoso.sharepoint.com/sites/HR/Benefits" }
      ]
    },
    {
      "name": "WebSearch"
    }
  ],
  "actions": [
    {
      "id": "hrisApi",
      "file": "api-plugins/ai-plugin.json"
    }
  ],
  "conversation_starters": [
    { "text": "What is our PTO policy?" },
    { "text": "How do I enroll in benefits?" },
    { "text": "What holidays does Contoso observe?" }
  ]
}
```

### instructions.md (System Prompt)
```markdown
You are the Contoso HR assistant. Your role is to help employees
with questions about company policies, benefits, PTO, and HR processes.

Rules:
- Always cite the specific policy document when answering
- If unsure, say so and direct the employee to HR@contoso.com
- Never share salary information or confidential employee data
- Be professional but friendly

When looking up information:
- Check the HR Policies library first
- Then check the Benefits library
- Use web search only for general HR concepts, not company-specific answers
```

## Testing and Deployment

### Local Testing
Use Agents Toolkit's sideloading to test in M365 Copilot before publishing.

### Publishing
1. **Sideload** — Test in your tenant (Agents Toolkit handles this)
2. **Admin approval** — Submit to org app catalog
3. **Microsoft Store** — Publish for broader distribution (optional)

## API Plugins

Declarative agents call APIs via OpenAPI specs:

```yaml
# api-plugins/openapi.yml
openapi: 3.0.0
info:
  title: HR API
  version: 1.0.0
servers:
  - url: https://api.contoso.com/hr
paths:
  /employees/{id}/pto:
    get:
      operationId: getPTOBalance
      summary: Get PTO balance for an employee
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
```

## Adaptive Cards

Render rich interactive responses:
```json
{
  "type": "AdaptiveCard",
  "body": [
    { "type": "TextBlock", "text": "PTO Balance", "weight": "Bolder" },
    { "type": "TextBlock", "text": "15 days remaining" },
    { "type": "ActionSet", "actions": [
      { "type": "Action.OpenUrl", "title": "Request PTO", "url": "https://hr.contoso.com/pto" }
    ]}
  ]
}
```

## Best Practices

1. Write clear, scoped instructions — tell the agent what NOT to do
2. Limit knowledge sources — too many leads to poor grounding
3. Use conversation starters to guide users
4. Test with real user questions
5. Version control the manifest files
6. Always verify current manifest schema via `microsoft_docs_search`
