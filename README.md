# Policy Compliance Co-Pilot

> **Agents League Hackathon — Enterprise Agents Track**

NOTE:  **See [Policy Compliance Co-Pilot Deck](https://htmlpreview.github.io/?https://github.com/i-b1/Compliance_Co-Pilot/blob/main/docs/presentation.html) for full interactive presentation.**

Ask Copilot Chat _"Is our new feature GDPR-compliant?"_ and get a side-by-side gap analysis between the regulator's rules and your company's internal policy — with a one-click action to file a compliance issue on GitHub.

---

## Why This Exists

Compliance teams drown in cross-referencing regulator text against internal policy documents. Engineers ship features without knowing a policy gap exists. Audit trails are manual and slow.

**Compliance Co-Pilot** solves this by wiring two AI knowledge bases (one per source of truth) to an orchestrator agent that diffs their answers and presents the result as an actionable Adaptive Card — no custom backend, no code to maintain.

---

## Architecture at a Glance

```
M365 Copilot Chat
        │
        ▼
┌─────────────────────────────────────┐
│  Compliance Co-Pilot  (Orchestrator) │
│  Copilot Studio                      │
└────┬──────────────┬──────────────┬───┘
     │              │              │
     ▼              ▼              ▼
Regulation IQ   Internal       GitHub MCP
(Foundry IQ)    Policy IQ      (OAuth)
GDPR/HIPAA/     Company        create_issue
SOC2 PDFs       SharePoint     get_file_contents
                policies       search_code
```

Full architecture details and Mermaid diagram: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)

---

## Hackathon Rubric

| Criterion | Weight | Evidence |
|---|---|---|
| **Multi-step reasoning** | 20% | Orchestrator calls both sub-agents independently, then synthesizes a structured gap analysis diff |
| **Accuracy** | 20% | Both sub-agents are grounded in Foundry IQ knowledge bases with general knowledge disabled; every answer cites an exact clause |
| **Creativity** | 15% | Two AI agents debate a compliance question, then the result is filed as a GitHub issue — zero custom backend code |
| **UX** | 15% | Adaptive Card shows "Regulator says / We say / Gap" with one-click GitHub action button |
| **Reliability / Safety** | 20% | OAuth handled by GitHub itself; only 5 least-privilege tools enabled; Copilot Studio content moderation at high |
| **Community** | 10% | Public repo, MIT license, recorded demo video |

---

## Key Demo Scenario

**Prompt:** _"Does our data breach notification process comply with GDPR?"_

**What happens:**
1. Orchestrator calls **Regulation IQ** → returns: _"GDPR Article 33 requires notification within **72 hours**."_
2. Orchestrator calls **Internal Policy IQ** → returns: _"Our Data Incident Policy §4.2 requires notification within **5 business days**."_
3. Orchestrator diffs the answers → **HIGH severity gap** identified.
4. Adaptive Card renders the side-by-side comparison.
5. User clicks **"File GitHub Issue"** → OAuth pop-up → issue `[HIGH] GDPR Art.33 breach notification gap` opened.

---

## Repository Structure

```
agents/
├── README.md
├── CLAUDE.md                  ← development instructions for Claude Code
├── docs/
│   ├── idea.md
│   └── ARCHITECTURE.md
├── IB-Agent/                  ← Copilot Studio orchestrator YAML
│   ├── agent.mcs.yml
│   ├── settings.mcs.yml
│   └── topics/
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

---

## Connected Agents & Tools

### Sub-agent: Regulation IQ
- **Knowledge:** Foundry IQ → SharePoint `Compliance/Regulations/` (GDPR, HIPAA, SOC2 PDFs)
- **Instructions:** Quote exact clause + article/section. Never speculate. General knowledge disabled.

### Sub-agent: Internal Policy IQ
- **Knowledge:** Foundry IQ → SharePoint `Compliance/InternalPolicies/` (company DOCXs)
- **Instructions:** Quote exact clause + document title. General knowledge disabled.

### GitHub MCP Server (Remote, OAuth)
- **Endpoint:** `https://api.githubcopilot.com/mcp/`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.0 — user-identity (single sign-in per user, token stored by Copilot Studio)
- **Enabled tools:** `create_issue`, `get_issue`, `list_issues`, `search_code`, `get_file_contents`

---

## Getting Started

> **Prerequisites:** Microsoft 365 Copilot license, Copilot Studio access, GitHub account, VS Code with Copilot Studio extension.

1. Clone this repository.
2. Open the `agents/` folder in VS Code.
3. Use the Copilot Studio extension to import the agent YAML files.
4. Upload PDFs from `knowledge/regulations/` and DOCXs from `knowledge/internal-policies/` to SharePoint.
5. Connect the Foundry IQ knowledge sources in each sub-agent.
6. Add the GitHub MCP server in the orchestrator's Tools panel.
7. Publish and test in M365 Copilot Chat.

---

## License

MIT
