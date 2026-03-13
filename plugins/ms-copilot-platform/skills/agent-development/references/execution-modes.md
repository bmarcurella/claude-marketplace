# Execution Modes — Guided vs Autonomous

After the Agent Design Plan is approved, execution proceeds in one of two modes.
Always ask the user which mode they prefer. Default to Guided Walkthrough.

## Mode A: Guided Walkthrough (Default)

### Purpose
Teach while building. Every step includes explanation of what, why, and how.
Ideal for learning, first-time builds, and teams building internal capability.

### How It Works

For each step in the Build Sequence:

```
Step [N.M]: [Action Name]
─────────────────────────

📋 WHAT: [What is being created or configured]

🎯 WHY:  [Why this matters in the architecture]
         [How it connects to other components]

🔧 HOW:
   [Exact commands, file contents, or UI steps]
   [Show the actual code/config, not just descriptions]

✅ VERIFY:
   [Command or action to confirm success]
   [Expected output or behavior]

💡 LEARN:
   [Architecture concept this demonstrates]
   [Link to Microsoft Learn docs for deeper reading]

⚠️ TROUBLESHOOT:
   [Common failure modes and fixes]
```

### Pacing
- Present one step at a time
- Wait for user confirmation before proceeding to next step
- Offer to explain concepts in more depth if asked
- Skip explanation for concepts the user already understands

### Example: Guided Walkthrough Step

```
Step 2.1: Scaffold the Agent Project
─────────────────────────────────────

📋 WHAT: Create the initial project structure for a Python custom
   engine agent using the Agents SDK with Semantic Kernel.

🎯 WHY:  The project scaffold sets up the foundational files —
   the entry point (app.py), the agent handler (agent.py),
   the Bicep infrastructure templates, and the M365 app manifest.
   Getting the structure right from the start makes everything
   downstream easier.

🔧 HOW:
   Option 1: Via Agents Toolkit (VS Code)
   1. Open VS Code Command Palette (Ctrl+Shift+P)
   2. Select "Agents: Create a New Agent/App"
   3. Choose "Custom Engine Agent"
   4. Select "Python"
   5. Select "Semantic Kernel" as orchestration
   6. Name: "contoso-support-agent"

   Option 2: Via CLI
   teamsfx new --interactive false \
     --app-type custom-engine-agent \
     --programming-language python \
     --ai-service semantic-kernel \
     --folder ./contoso-support-agent

✅ VERIFY:
   ls contoso-support-agent/src/
   # Expected: app.py, agent.py, config.py

   python -c "import microsoft_agents; print('SDK installed')"
   # Expected: "SDK installed"

💡 LEARN:
   The ActivityHandler pattern is how Microsoft's agent framework
   works. Your agent receives "activities" (messages, events, etc.)
   and responds. Think of it as the event loop for your agent.
   Learn more: https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/

⚠️ TROUBLESHOOT:
   - "Agents Toolkit not found" → Install the Microsoft 365 Agents
     Toolkit extension from VS Code marketplace
   - "Python version error" → Agents SDK requires Python >= 3.10
   - "Permission denied" → Run terminal as administrator
```

## Mode B: Autonomous Build (Claude Code)

### Purpose
Generate all artifacts at once. The user gets a complete, deployable project.
Ideal for experienced developers, rapid prototyping, and CI/CD pipelines.

### How It Works

Generate the complete project in a single pass:

1. **Create project directory** with full structure
2. **Write all source files** — agent code, tools, configs
3. **Generate Bicep templates** for infrastructure
4. **Create Dockerfile and docker-compose.yml**
5. **Generate app manifest** for M365 registration
6. **Write test files** with basic test coverage
7. **Create README.md** with architecture explanation and setup instructions
8. **Generate deployment scripts** — deploy.sh, cleanup.sh
9. **Create .env.example** with all required environment variables

### Output Structure

```
[agent-name]/
├── README.md                # Architecture explanation + setup guide
├── src/
│   ├── app.py               # Entry point with full configuration
│   ├── agent.py             # Agent handler with AI integration
│   ├── config.py            # Environment and settings management
│   └── tools/
│       ├── __init__.py
│       └── [domain]_tools.py  # Domain-specific tools
├── requirements.txt         # All Python dependencies
├── Dockerfile               # Production container definition
├── docker-compose.yml       # Local dev with all dependencies
├── .env.example             # Template for environment variables
├── appPackage/
│   ├── manifest.json        # M365 app manifest (ready to submit)
│   ├── outline.png
│   └── color.png
├── infra/
│   ├── main.bicep           # Complete Azure infrastructure
│   ├── main.parameters.json
│   └── modules/
│       ├── app-service.bicep
│       ├── bot-service.bicep
│       ├── keyvault.bicep
│       ├── identity.bicep
│       └── monitoring.bicep
├── scripts/
│   ├── deploy.sh            # One-command deployment
│   ├── cleanup.sh           # Teardown all resources
│   └── setup-local.sh       # Local environment setup
├── tests/
│   ├── test_agent.py
│   └── test_tools.py
└── .github/
    └── workflows/
        └── deploy.yml       # CI/CD pipeline
```

### README Content Requirements

The generated README must include:
1. **Architecture Overview** — Mermaid diagram from the design plan
2. **Prerequisites** — Exact versions of tools needed
3. **Local Setup** — Step-by-step from clone to running
4. **Environment Variables** — Every variable explained
5. **Deployment** — How to deploy to Azure
6. **Testing** — How to run tests
7. **Architecture Decisions** — Why each technology was chosen
8. **Cleanup** — How to tear everything down
9. **Cost Estimate** — Monthly cost breakdown

### Quality Checklist for Autonomous Builds

Before presenting the generated project, verify:
- [ ] All files are syntactically valid
- [ ] Requirements.txt includes all imported packages
- [ ] Dockerfile builds successfully (dry run)
- [ ] Bicep templates validate (`az bicep build`)
- [ ] .env.example has every required variable
- [ ] No secrets are hardcoded anywhere
- [ ] README accurately describes the architecture
- [ ] Tests are meaningful (not just pass-through)
- [ ] Cleanup script removes everything created

## Mode C: Hybrid (Recommended for Most Users)

### How It Works
- **Autonomous** for infrastructure scaffolding, boilerplate, and configuration
- **Guided** for agent logic, custom instructions, and tool implementation

This gives users the speed of automation for repetitive parts while
maintaining the learning experience for the unique parts of their agent.

### Hybrid Flow
1. Autonomously generate: project scaffold, Bicep, Docker, deployment scripts
2. Guide through: AI configuration, custom instructions, tool implementation
3. Autonomously generate: tests, CI/CD pipeline, monitoring setup
4. Guide through: first deployment, testing, Agent 365 registration

## Choosing the Right Mode

| Factor | Guided | Autonomous | Hybrid |
|--------|--------|------------|--------|
| Learning goal | ★★★ | ★ | ★★ |
| Speed | ★ | ★★★ | ★★ |
| First-time builder | ★★★ | ★ | ★★ |
| Experienced developer | ★ | ★★★ | ★★ |
| Team enablement | ★★★ | ★ | ★★★ |
| Production readiness | ★★ | ★★★ | ★★★ |
