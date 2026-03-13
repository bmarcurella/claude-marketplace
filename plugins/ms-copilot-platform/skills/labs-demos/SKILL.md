---
name: labs-demos
description: >
  This skill should be used when the user asks to "create a lab", "build a demo", "hands-on
  walkthrough", "practice building agents", "learning exercise", "tutorial for Copilot",
  "step-by-step guide", "create a workshop", "training materials", "demo environment",
  "sandbox setup", "practice with agents", "learn Microsoft agents", "agent workshop",
  "build a proof of concept", "POC for agents", "try out Copilot Studio", "experiment with
  Agents SDK", or any variation of wanting to learn, practice, explore, or demonstrate
  Microsoft agent platform capabilities through hands-on exercises. Also triggers for creating
  training curricula, workshop agendas, and learning paths.
---

# Labs, Demos & Learning Experiences

This skill generates hands-on learning experiences for the Microsoft Frontier Agent Platform.
It creates structured labs, demos, walkthroughs, and workshops tailored to the user's skill
level and learning objectives.

## Lab Generation Workflow

### Step 1: Assess the Learner

Determine:
- **Current skill level**: No-code, low-code, pro-code, architect
- **Learning objective**: What specific capability or concept to learn
- **Available tools**: What licenses, environments, and access the learner has
- **Time available**: Quick demo (15 min), lab (1-2 hrs), workshop (half/full day)

### Step 2: Select Lab Template

Choose from `references/lab-templates.md` based on the learning objective:

| Category | Labs Available | Skill Level |
|----------|---------------|-------------|
| **Getting Started** | First Agent, Agent Builder, SharePoint Agent | Beginner |
| **Copilot Studio** | Topics, Connectors, Autonomy, Publishing | Beginner-Intermediate |
| **Declarative Agents** | Manifest, Knowledge, Actions, Adaptive Cards | Intermediate |
| **Custom Engine** | SDK Setup, Orchestration, Multi-Channel, A2A | Advanced |
| **Foundry** | Agent Service, Model Deployment, Evaluations | Advanced |
| **Governance** | Agent 365 Setup, Policies, Monitoring | Intermediate-Advanced |
| **Integration** | MCP Servers, Connectors, Graph API, Fabric | Intermediate-Advanced |
| **Architecture** | Solution Design, Multi-Agent, Enterprise Scale | Advanced |

### Step 3: Generate the Lab

Each lab includes:

1. **Overview** — What the learner will build and why it matters
2. **Prerequisites** — Licenses, tools, environment setup
3. **Architecture Diagram** — Visual showing what's being built and how it fits the platform
4. **Step-by-Step Instructions** — Numbered steps with code/config/screenshots
5. **Checkpoints** — Verification steps to confirm progress
6. **Extension Challenges** — Optional advanced exercises
7. **Cleanup** — How to remove resources when done
8. **Key Takeaways** — What was learned and how it applies to real scenarios

### Step 4: Provide Supporting Materials

- **Starter files** — Templates, manifests, configs to begin with
- **Solution files** — Complete working examples for reference
- **Architecture context** — How the lab fits into the broader platform architecture
- **Next steps** — What to learn next after completing the lab

## Demo Generation

For demos (shorter, presentation-focused):

1. **Scenario description** — Business context and problem statement
2. **Demo script** — Talking points with specific clicks and interactions
3. **Setup instructions** — Pre-demo configuration
4. **Wow moments** — Key highlights that showcase platform capabilities
5. **Q&A preparation** — Common questions and answers

## Learning Paths

Structured multi-lab curricula. Read `references/learning-paths.md` for:

### Path 1: Citizen Developer (2-3 days)
Agent Builder → Copilot Studio basics → Connectors → Publishing → Governance basics

### Path 2: Pro Developer (3-5 days)
Declarative agents → API plugins → Custom engine → Agents SDK → Foundry → A2A

### Path 3: Architect (2-3 days)
Platform overview → Solution design → Integration patterns → Governance → Scale

### Path 4: Full Stack (5-7 days)
All of the above, building a complete multi-agent enterprise solution

## Workshop Generation

For instructor-led workshops, generate:
- Agenda with time blocks
- Presenter slides outline
- Participant lab guides
- Environment setup guide
- Facilitation notes

## Best Practices for Lab Design

1. **Always show the "why"** — Connect every exercise to a real business scenario
2. **Architecture-first** — Show where this fits in the platform before diving into steps
3. **Checkpoint often** — Learners should verify progress every 5-10 steps
4. **Fail gracefully** — Include troubleshooting tips for common errors
5. **Progressive complexity** — Start simple, layer on capabilities
6. **Clean up** — Always include resource cleanup instructions

## Using Microsoft Learn MCP

Use `microsoft_docs_search` and `microsoft_docs_fetch` to:
- Pull the latest quickstart guides and tutorials
- Verify current UI paths and menu options (these change frequently)
- Get current sample code and project templates
- Check prerequisite requirements and licensing

## Reference Files

- **`references/lab-templates.md`** — Detailed templates for each lab category
- **`references/learning-paths.md`** — Multi-lab curricula for each audience
- **`references/environment-setup.md`** — Environment and licensing setup guides
- **`references/demo-scripts.md`** — Pre-built demo scenarios and scripts
