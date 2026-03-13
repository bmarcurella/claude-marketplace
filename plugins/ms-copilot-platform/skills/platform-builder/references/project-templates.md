# Project Templates Reference

This file cross-references the project templates defined in the agent-development skill.

For complete template definitions, consult:
`agent-development/references/project-templates.md`

## Quick Reference

| Template | Type | Language | Framework |
|----------|------|----------|-----------|
| T1 | Custom Engine | Python | Semantic Kernel |
| T2 | Custom Engine | Python | LangChain |
| T3 | Custom Engine | C# | Semantic Kernel |
| T4 | Custom Engine | TypeScript | Semantic Kernel |
| T5 | Declarative | JSON | Agents Toolkit |
| T6 | Copilot Studio | Solution | Power Platform |
| T7 | MCP Server | Python | FastMCP |
| T8 | Multi-Agent | Python | Mixed (A2A) |

## Usage in Build Process

1. Identify the correct template from the Agent Design Plan
2. Generate scaffold using the template
3. Customize for the specific domain
4. Add infrastructure (Bicep modules from iac-templates.md)
5. Configure deployment (scripts from agent-scaffolds.md)
