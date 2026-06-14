# This is an enterprise agent for the Agents League Hackathon.

The rules, bonus criteria and evaluation summary are provided in this URL: [**https://github.com/carlotta94c/agentsleague/blob/main/starter-kits/3-enterprise-agents/README.md**](https://github.com/carlotta94c/agentsleague/blob/main/starter-kits/3-enterprise-agents/README.md)

**Important**: The agent is developed using Copilot studio extension for VS Code and “copilot-studio@skills-for-copilot-studio" plugin for Claude Code and GitHub Copilot that lets you author, test, and troubleshoot Copilot Studio agents directly from your terminal. 
The plugin information is in this repo: [Skills for Copilot Studio](https://github.com/microsoft/skills-for-copilot-studio).
To Create and edit agent YAML (topics, actions, knowledge, triggers, variables) you must use slash command '/copilot-studio:copilot-studio-author'

## My initial idea: **"Policy Compliance Co-Pilot" (Legal & Compliance)**

- **Orchestrator + 2 connected agents:**
    - **Regulation Agent (Foundry IQ):** GDPR / HIPAA / SOC2 documents.
    - **Internal Policy Agent (Foundry IQ):** company SharePoint policy library.
- **MCP (OAuth):** GitHub MCP — scans a repo's README for compliance disclosures and opens a PR with redlines.
- **Adaptive Card:** side-by-side "Regulator says / We say / Gap" with "Open issue" action.

## 1\. 🎯 Concept & Value Proposition

**Elevator pitch:** _"Ask Copilot Chat 'Is our new feature GDPR-compliant?' and get a side-by-side gap analysis between the regulator's rules and your company's internal policy — with a one-click action to file a compliance issue or open a redline PR."_

**Target users:** Legal, Privacy, Risk, Engineering Leads, Product Managers.

**Pain points solved:**

- Compliance teams drown in cross-referencing regulator text vs. internal policy.
- Engineers ship features without realizing a policy gap exists.
- Audit trails are manual and slow.

**Core differentiator:** Two **Foundry IQ** knowledge bases reasoning _against each other_ (regulation ↔ internal policy), orchestrated by a third agent that also writes findings to GitHub via an OAuth MCP server.

## 3\. 🏗️ Solution Architecture

```yaml
┌──────────────────── M365 Copilot Chat ────────────────────┐
│                                                           │
│   User: "Does our data export feature comply with GDPR?"  │
│                            │                              │
│                            ▼                              │
│   ┌────────────────────────────────────────────────────┐  │
│   │   ORCHESTRATOR AGENT  (Copilot Studio)             │  │
│   │   "Compliance Co-Pilot"                            │  │
│   │   - Triages question                               │  │
│   │   - Calls Regulation Agent                         │  │
│   │   - Calls Internal Policy Agent                    │  │
│   │   - Diffs answers → builds Adaptive Card           │  │
│   │   - Calls GitHub MCP for action                    │  │
│   └─────────┬───────────────┬──────────────┬──────────┘   │
│             │               │              │              │
│   ┌─────────▼─────┐ ┌───────▼──────┐ ┌────▼────────────┐  │
│   │ REGULATION    │ │ INTERNAL     │ │ GITHUB MCP      │  │
│   │ AGENT         │ │ POLICY AGENT │ │ SERVER (OAuth)  │  │
│   │ (Foundry IQ)  │ │ (Foundry IQ) │ │  (Remote)       │  │
│   │               │ │              │ │                 │  │
│   │ KB: GDPR,     │ │ KB: SharePt  │ │ Tools:          │  │
│   │ HIPAA, SOC2   │ │ policies,    │ │ - read_readme   │  │
│   │ (PDFs)        │ │ standards    │ │ - open_issue    │  │
│   │               │ │ (DOCX/PDF)   │ │ - create_pr     │  │
│   └───────────────┘ └──────────────┘ └─────────────────┘  │
└───────────────────────────────────────────────────────────┘
```

**Why this wins:** Connected Agents + dual Foundry IQ + MCP write-action + Adaptive Card = every bonus box ticked, while the demo story ("two AIs debate, then file a GitHub issue") is memorable.

## 4\. 📁 Sample Repository Layout
```yaml
policy-compliance-copilot/
├── README.md
├── LICENSE
├── docs/
│   ├── architecture.png
│   ├── demo.gif
│   └── screenshots/
├── copilot-studio/
│   ├── orchestrator-agent.zip
│   ├── regulation-agent.zip
│   └── internal-policy-agent.zip
├── knowledge/
│   ├── regulations/
│   │   ├── gdpr-summary.pdf
│   │   ├── hipaa-privacy-rule.pdf
│   │   └── soc2-trust-criteria.pdf
│   └── internal-policies/
│       ├── data-handling-policy.docx
│       ├── retention-standard.docx
│       └── access-control-baseline.docx
├── adaptive-cards/
│   └── gap-analysis-card.json
└── demo/
    ├── script.md
    └── video-link.txt
```

## 5\. 🚀 Step-by-Step Plan

### Phase 1 — Knowledge Base Prep

1.  SharePoint folder **Compliance/Regulations/** → drop GDPR / HIPAA / SOC2 PDFs.
2.  SharePoint folder **Compliance/InternalPolicies/** → 3 fake DOCX policies.
3.  **Plant an intentional gap** (e.g., GDPR 72h vs internal 5 business days).

### Phase 2 — Build Two Sub-Agents

**Sub-agent A: "Regulation Agent"**

1.  Copilot Studio → Create → New agent → name it **Regulation IQ**.
2.  Description: _"Answers questions about external regulations (GDPR, HIPAA, SOC2). Always cites the article/section."_
3.  **Knowledge → Add knowledge → SharePoint** → point at **Compliance/Regulations/**. This becomes a Foundry IQ-powered knowledge source.
4.  Instructions: _"You are a regulatory expert. For every answer, quote the exact clause and cite document + section. Never speculate beyond cited text."_
5.  Disable "general knowledge" so it stays grounded.
6.  Test in the side panel: _"What does GDPR say about breach notification timing?"_ → must cite Article 33.

**Sub-agent B: "Internal Policy Agent"**

- Same recipe, pointed at **Compliance/InternalPolicies/**.
- Instructions: _"Answer only from internal company policies. Quote exact clause + doc title."_

### Phase 3 — Build Orchestrator + Connected Agents

1.  New agent: **Compliance Co-Pilot**.
2.  **Description (critical for orchestration):** _"A compliance assistant that compares external regulations to internal policies, identifies gaps, and files GitHub issues for remediation."_
3.  **Add Connected Agents:**
    1.  Settings → Agents → **Add agent** → select **Regulation IQ**.
        1.  Hand-off description: _"Use when the user asks what a regulator (GDPR, HIPAA, SOC2) requires."_
    2.  Add **Internal Policy Agent**.
        1.  Hand-off description: _"Use when the user asks what our company policy says."_
4.  Top-level instructions:

You are the Compliance Co-Pilot. For any compliance question:  
1) Call Regulation IQ to get the regulator's stance (with citation).  
2) Call Internal Policy IQ to get our internal stance (with citation).  
3) Compare them and produce a structured gap analysis with:  
topic, regulator_clause, internal_clause, gap_severity (LOW/MED/HIGH), recommendation.  
4) Render the result using the "Gap Analysis" Adaptive Card.  
5) If severity is MED or HIGH, offer to file a GitHub issue using the GitHub tool.  
6) When the user confirms, call create_issue on the GitHub MCP with:  
\- owner/repo from the user (or default to the configured demo repo)  
\- title prefixed with \[SEVERITY\]  
\- body containing both citations and the recommendation  
\- labels: \["compliance", severity.toLowerCase()\]  
Always cite sources. Never invent clauses.  

### Phase 4 — ⭐ Connect the Remote GitHub MCP Server

This is the new, simpler flow.

1.  In Copilot Studio, open your **orchestrator** (**Compliance Co-Pilot**).
2.  **Tools → + Add a tool → Model Context Protocol → Github MCP server**.

**Note**: alternatively use built-in Github MCP with oauth connection.

1.  Fill in:

|     |     |
| --- | --- |
| **Field** | **Value** |
| Server name | **GitHub** |
| Server description | **Official GitHub remote MCP server — issues, PRs, code search, repos.** |
| Server URL | [**https://api.githubcopilot.com/mcp/**](https://api.githubcopilot.com/mcp/) |
| Transport | **Streamable HTTP** |
| Authentication | **OAuth 2.0** |

1.  For OAuth, Copilot Studio offers two patterns:
    1.  **Easy path (recommended for hackathon):** select **"Authenticate with the user's identity"** — Copilot Studio brokers the GitHub OAuth handshake on first use; the user clicks "Sign in to GitHub" once in chat.
    2.  **Power path:** register your own OAuth app to get branded consent screens. Not needed for the demo.
2.  Click **Create**. Copilot Studio will probe the endpoint and auto-discover the tool catalog (issues, pulls, repos, code-search, security, actions, gists, notifications, etc.).
3.  **Enable only the tools you need** to keep the agent focused (best practice — fewer tools = better routing accuracy):
    1.  ✅ **create_issue**
    2.  ✅ **get_issue**
    3.  ✅ **list_issues**
    4.  ✅ **search_code**
    5.  ✅ **get_file_contents** (this is your "read README" tool)
    6.  ✅ **create_pull_request** (stretch demo)
    7.  ⬜ Disable everything else
4.  Click **Save**. Publish the agent.

### Phase 5 — Adaptive Card UI

Same **gap-analysis-card.json** as before. The button payload now references the remote MCP tool name:

"actions": \[  
{ "type": "Action.Submit", "title": "📌 File GitHub issue",  
"data": {  
"action": "create_issue",  
"severity": "${severity}",  
"title": "${topic}",  
"body": "${recommendation}"  
}  
},  
{ "type": "Action.Submit", "title": "✅ Mark as accepted risk",  
"data": { "action": "accept_risk" } }  
\]  

In the orchestrator topic that handles the card submit, route **action == "create_issue"** straight into the GitHub MCP **create_issue** tool.

### Phase 6 — Test, Record, Submit

Same demo script and submission checklist as before.

## 6\. 🧪 First-Run User Experience

When a user triggers a GitHub action for the first time in Copilot Chat:

User: File a GitHub issue for that GDPR gap.  
<br/>Copilot: Before I can do that, please sign in to GitHub.  
\[ Sign in to GitHub \] ← OAuth consent in popup  
<br/>(After consent — token stored per user by Copilot Studio)  
<br/>Copilot: ✅ Issue #42 opened in yourname/compliance-demo  
🔗 https://github.com/yourname/compliance-demo/issues/42  

The token is stored per-user by Copilot Studio, so subsequent calls are silent. This is exactly the OAuth experience the hackathon judges want to see.  
<br/>7\. 📝 README Talking Points

**In your repo's README.md, lead with these bullets — they map to the rubric:**

- **Multi-step reasoning (20%): Two Foundry IQ knowledge bases reason against each other; orchestrator synthesizes a structured diff.**
- **Accuracy (20%): Every claim is grounded in a cited clause — regulator and internal — with no general-knowledge fallback.**
- **Creativity (15%): "Two AIs debate, then file a GitHub issue" — and we shipped it without writing or hosting any custom backend.**
- **UX (15%): Adaptive Card delivers the gap analysis; one-click action to GitHub.**
- **Reliability/Safety (20%): OAuth handled by GitHub itself; least-privilege tool selection (only 5 tools enabled); content moderation high.**
- **Community (10%): Public repo, MIT license, demo video, Discord post.**
