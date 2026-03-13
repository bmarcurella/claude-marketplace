# Orchestration Frameworks Comparison

Guide for choosing and implementing AI orchestration frameworks with Microsoft custom engine agents.

## Framework Comparison

### Semantic Kernel (Microsoft)

**Best for:** M365-integrated agents, Azure OpenAI, enterprise environments

**Strengths:**
- Native Microsoft integration — first-class Azure OpenAI support
- Plugin system aligns with M365 Copilot plugin model
- Memory management (conversation, semantic, entity)
- Planners for multi-step task decomposition
- Filters for pre/post function call hooks (logging, auth, safety)
- .NET, Python, and Java SDKs

**Setup (Python):**
```
pip install semantic-kernel azure-identity
```

**When to choose:** Building agents that heavily integrate with M365 services,
teams already invested in Azure/Microsoft ecosystem, need for enterprise-grade
function calling with filters and memory.

### LangChain / LangGraph

**Best for:** Complex reasoning chains, multi-agent graphs, RAG-heavy agents

**Strengths:**
- Largest open-source AI framework community
- LangGraph for stateful multi-step agent workflows
- Extensive retrieval/RAG tooling
- Model-agnostic (works with any LLM provider)
- LangSmith for tracing and debugging
- Rich ecosystem of pre-built tools and integrations

**Setup (Python):**
```
pip install langchain langchain-openai langgraph
```

**When to choose:** Complex multi-step reasoning, heavy RAG requirements,
teams with existing LangChain experience, need for sophisticated agent
state machines (via LangGraph).

### OpenAI Agents SDK

**Best for:** Simple tool-calling agents, OpenAI-model-centric projects

**Strengths:**
- Simple API — minimal boilerplate
- Native OpenAI function calling
- Built-in tool loop (agent keeps calling tools until done)
- Handoff pattern for multi-agent
- Guardrails (input/output validation)

**Setup:**
```
pip install openai-agents
```

**When to choose:** Straightforward tool-calling agents, teams standardized
on OpenAI models, want minimal framework overhead.

### Crew.ai

**Best for:** Multi-agent teams with role-based specialization

**Strengths:**
- Role-based agent definitions (each agent has a role, goal, backstory)
- Sequential and parallel task execution
- Built-in delegation between agents
- Process management (sequential, hierarchical, consensual)
- Memory and knowledge tools

**Setup:**
```
pip install crewai crewai-tools
```

**When to choose:** Multi-agent scenarios where agents have distinct roles,
team-based workflows (e.g., researcher + analyst + writer).

### AutoGen (Microsoft Research)

**Best for:** Research, experimentation, multi-agent conversation

**Strengths:**
- Multi-agent conversation framework
- Code execution capabilities
- Human-in-the-loop patterns
- Group chat orchestration
- Custom agent definitions

**Setup:**
```
pip install autogen-agentchat
```

**When to choose:** Research and experimentation, complex multi-agent
conversations, code generation and execution workflows.

## Integration with Agents SDK

All frameworks integrate with the Microsoft 365 Agents SDK the same way —
the framework handles AI orchestration, the SDK handles channel communication:

```
User (Teams/Copilot/Web)
    ↓
Microsoft 365 Agents SDK (ActivityHandler)
    ↓
Orchestration Framework (SK / LangChain / etc.)
    ↓
AI Model (Azure OpenAI / etc.)
    ↓
Tools / Actions
```

### Integration Pattern (Any Framework)

```python
class MyAgent(ActivityHandler):
    def __init__(self, orchestrator):
        # orchestrator = Semantic Kernel / LangChain / etc.
        self.orchestrator = orchestrator

    async def on_message_activity(self, turn_context):
        user_message = turn_context.activity.text

        # Delegate to framework
        response = await self.orchestrator.process(user_message)

        await turn_context.send_activity(response)
```

## Decision Matrix

| Factor | Semantic Kernel | LangChain | OpenAI Agents | Crew.ai | AutoGen |
|--------|:-:|:-:|:-:|:-:|:-:|
| **M365 Integration** | ★★★ | ★★ | ★ | ★ | ★ |
| **Multi-agent** | ★★ | ★★★ | ★★ | ★★★ | ★★★ |
| **RAG Quality** | ★★ | ★★★ | ★ | ★★ | ★★ |
| **Simplicity** | ★★ | ★ | ★★★ | ★★ | ★ |
| **Community** | ★★ | ★★★ | ★★ | ★★ | ★★ |
| **Enterprise Ready** | ★★★ | ★★ | ★★ | ★ | ★ |
| **Debugging Tools** | ★★ | ★★★ | ★★ | ★ | ★ |

## Mixing Frameworks

Frameworks can be combined. Common patterns:

- **Semantic Kernel + LangChain:** SK for M365 function calling, LangChain for RAG
- **Semantic Kernel + Crew.ai:** SK for single-agent tools, Crew.ai for multi-agent orchestration
- **LangGraph + A2A:** LangGraph for complex agent graphs, A2A for cross-service agent communication

The Agents SDK is always the outer layer — frameworks are interchangeable inside it.
