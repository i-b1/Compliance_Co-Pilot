# CLAUDE.md — Compliance Co-Pilot (Agents League Hackathon)

## Project Overview

This is a **Copilot Studio** enterprise agent project for the Agents League Hackathon (Enterprise Agents track). The solution is a "Policy Compliance Co-Pilot" — an orchestrator agent backed by two Foundry IQ sub-agents and a GitHub MCP server.

## Repository Layout

```
agents/
├── CLAUDE.md                          ← you are here
├── README.md
├── docs/
│   ├── idea.md                        ← original design spec
│   └── ARCHITECTURE.md               ← architecture + Mermaid diagram
├── IB-Agent/                          ← Copilot Studio YAML (orchestrator)
│   ├── agent.mcs.yml
│   ├── settings.mcs.yml
│   └── topics/
├── knowledge/
│   ├── regulations/                   ← GDPR, HIPAA, SOC2 PDFs (Foundry IQ KB)
│   └── internal-policies/             ← company policy DOCXs (Foundry IQ KB)
├── adaptive-cards/
│   └── gap-analysis-card.json
└── demo/
    ├── script.md
    └── video-link.txt
```

## Agents in This Solution

| Agent | Role | Knowledge Source |
|---|---|---|
| **Compliance Co-Pilot** (orchestrator) | Triages questions, calls sub-agents, diffs answers, renders Adaptive Card, calls GitHub MCP | None (pure orchestration) |
| **Regulation IQ** (sub-agent) | Answers questions about GDPR / HIPAA / SOC2, always cites article/section | Foundry IQ → SharePoint `Compliance/Regulations/` |
| **Internal Policy IQ** (sub-agent) | Answers questions about internal company policies, always cites doc/clause | Foundry IQ → SharePoint `Compliance/InternalPolicies/` |

## Authoring Rules

### How to Edit Agents
- **Always use `/copilot-studio:copilot-studio-author`** (the Author sub-agent) to create or modify topics, actions, knowledge sources, variables, or triggers.
- Use `/copilot-studio:copilot-studio-advisor` to review YAML, troubleshoot validation errors, or get design guidance — not for authoring.
- Use `/copilot-studio:copilot-studio-manage` to sync (push/pull), publish, or deploy agents.
- Use `/copilot-studio:copilot-studio-test` to run evaluations and batch test suites.

### Topic Naming Convention
- System topics keep their default names (Greeting, Fallback, Escalate, etc.)
- Custom topics: `PascalCase` with a clear domain prefix, e.g. `ComplianceCheck`, `GapAnalysis`, `GitHubIssueCreate`

### Variable Naming
- Global variables: `G_<Name>` (e.g. `G_RepoOwner`, `G_RepoName`)
- Topic-scoped: `T_<Name>` (e.g. `T_RegulatorResponse`, `T_PolicyResponse`)

## Key Design Decisions

1. **Two Foundry IQ knowledge bases reason against each other** — regulator KB and internal-policy KB are intentionally separate agents so the orchestrator can collect independent, cited answers and diff them.
2. **GitHub MCP via OAuth** — uses the official `https://api.githubcopilot.com/mcp/` remote MCP endpoint with Streamable HTTP transport + user-identity OAuth. No custom backend required.
3. **Least-privilege tool selection** — only 5 GitHub tools are enabled: `create_issue`, `get_issue`, `list_issues`, `search_code`, `get_file_contents`.
4. **Adaptive Card for gap output** — the orchestrator renders a "Regulator says / We say / Gap" card; HIGH/MED gaps offer a one-click "File GitHub issue" action.
5. **Intentional compliance gap planted** — GDPR Article 33 requires 72h breach notification; internal policy document says "5 business days" — this is the key demo scenario.

## Environment & Tooling

- **Copilot Studio extension for VS Code** — local YAML authoring
- **Skills for Copilot Studio** plugin (`copilot-studio@skills-for-copilot-studio`) — Claude Code integration
- **M365 Copilot Chat** — runtime surface (Teams + Microsoft365Copilot channels enabled)
- Agent schema name: `cr47b_agent`
- Authentication: Integrated / Always (Microsoft Entra ID)

## Hackathon Rubric Checklist

| Criterion | Weight | How We Address It |
|---|---|---|
| Multi-step reasoning | 20% | Orchestrator calls both sub-agents, diffs, synthesizes gap analysis |
| Accuracy | 20% | Both sub-agents grounded in Foundry IQ KBs; general knowledge disabled |
| Creativity | 15% | "Two AIs debate, then file a GitHub issue" — no custom backend |
| UX | 15% | Adaptive Card with one-click GitHub action |
| Reliability / Safety | 20% | OAuth via GitHub, least-privilege tools, content moderation high |
| Community | 10% | Public repo, MIT license, demo video |

## Common Commands

```bash
# Pull latest agent state from Copilot Studio cloud
/copilot-studio:manage-agent pull

# Push local YAML changes to cloud
/copilot-studio:manage-agent push

# Publish draft → live
/copilot-studio:manage-agent publish

# Run a quick test conversation
/copilot-studio:chat-with-agent

# Validate YAML before pushing
/copilot-studio:validate
```
