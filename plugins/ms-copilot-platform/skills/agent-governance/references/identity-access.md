# Identity & Access Management

Entra ID integration patterns for agent identity, managed identities, app registrations,
OAuth flows, and least-privilege access for agents.

## Key Concepts

### Managed Identity (Recommended)
Use system-assigned or user-assigned managed identities for Azure-hosted agents.
Eliminates secret management — the identity is tied to the Azure resource.

### App Registration
Required for agents that authenticate users or call Graph API.
Always use the minimum required permissions.

### Authentication Flows
- **On-behalf-of (OBO)** — Agent acts on behalf of the signed-in user
- **Client credentials** — Agent acts as itself (service-to-service)
- **Device code** — For agents in environments without browser access

## Best Practices
1. Use managed identity for all Azure service-to-service calls
2. Store client secrets in Key Vault, never in code
3. Request minimum Graph permissions (prefer delegated over application)
4. Enable conditional access policies for agent app registrations
5. Rotate certificates annually
6. Use `microsoft_docs_search` to verify current auth patterns
