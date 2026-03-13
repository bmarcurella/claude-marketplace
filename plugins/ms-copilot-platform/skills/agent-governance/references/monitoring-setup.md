# Monitoring & Analytics Setup

Configure observability for agent solutions using Copilot Analytics, Application Insights,
and Azure Monitor.

## Copilot Analytics
Built-in analytics for agents running in M365 Copilot:
- Session volume and trends
- User satisfaction scores
- Topic usage and abandonment rates
- Knowledge source effectiveness
- Action success/failure rates

## Application Insights
For custom engine agents, configure App Insights for:
- Request tracing (end-to-end)
- Dependency tracking (AI model calls, API calls)
- Custom metrics (token usage, tool call frequency)
- Alerting on error rates and latency

## Key Metrics to Track
- Response latency (p50, p95, p99)
- Token consumption per conversation
- Tool call success rate
- User satisfaction (thumbs up/down)
- Conversation completion rate
- Error rate by category

## Dashboard Template
Create an Azure Dashboard with:
1. Agent health overview (availability, error rate)
2. Usage metrics (sessions, messages, unique users)
3. AI performance (latency, token usage)
4. Cost tracking (Azure resource costs)
