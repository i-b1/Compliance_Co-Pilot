# Architecture — Policy Compliance Co-Pilot

## Overview

The solution is a three-agent system built entirely on Microsoft Copilot Studio (no custom backend). An orchestrator agent delegates to two Foundry IQ-powered sub-agents, diffs their answers, and surfaces the result through an Adaptive Card with an optional GitHub write-action over OAuth MCP.

---

## System Diagram

```mermaid
flowchart TD
    User["👤 User\n(M365 Copilot Chat / Teams)"]

    subgraph CopilotStudio["Microsoft Copilot Studio"]
        Orch["🧭 Compliance Co-Pilot\nOrchestrator Agent\n─────────────────\n• Triages compliance question\n• Calls both sub-agents\n• Diffs answers → gap analysis\n• Renders Adaptive Card\n• Calls GitHub MCP on confirm"]

        subgraph SubAgents["Connected Sub-Agents (Foundry IQ)"]
            RegAgent["📋 Regulation IQ\nSub-Agent\n─────────────────\n• KB: GDPR / HIPAA / SOC2\n• Cites exact article/section\n• General knowledge OFF"]
            PolAgent["🏢 Internal Policy IQ\nSub-Agent\n─────────────────\n• KB: Company SharePoint\n• Cites doc title + clause\n• General knowledge OFF"]
        end

        AdaptiveCard["🃏 Gap Analysis\nAdaptive Card\n─────────────────\nRegulator says │ We say │ Gap\n[File Issue] [Accept Risk]"]
    end

    subgraph Foundry["Microsoft Foundry IQ"]
        RegKB["📚 Regulations KB\nSharePoint:\nCompliance/Regulations/\n• gdpr-summary.pdf\n• hipaa-privacy-rule.pdf\n• soc2-trust-criteria.pdf"]
        PolKB["📚 Internal Policies KB\nSharePoint:\nCompliance/InternalPolicies/\n• data-handling-policy.docx\n• retention-standard.docx\n• access-control-baseline.docx"]
    end

    subgraph GitHub["GitHub (Remote MCP Server)"]
        MCP["⚙️ GitHub MCP\nhttps://api.githubcopilot.com/mcp/\nStreamable HTTP + OAuth 2.0\n─────────────────\n✅ issue_write (create/update issues)\n✅ issue_read\n✅ list_issues\n✅ search_code\n✅ get_file_contents\n✅ search_issues"]
        Issues["🐛 GitHub Issues\nCompliance findings\nwith citations + labels"]
    end

    User -->|compliance question| Orch
    Orch -->|"1 · what does the regulator say?"| RegAgent
    Orch -->|"2 · what does our policy say?"| PolAgent
    RegAgent -->|grounded answer + citation| Orch
    PolAgent -->|grounded answer + citation| Orch
    Orch -->|"3 · render gap analysis"| AdaptiveCard
    AdaptiveCard -->|displayed to user| User
    User -->|"4 · file issue? (MED/HIGH only)"| Orch
    Orch -->|"5 · issue_write (OAuth)"| MCP
    MCP -->|issue created| Issues
    Issues -->|issue URL| Orch
    Orch -->|"✅ Issue #N opened"| User

    RegAgent <-->|semantic search| RegKB
    PolAgent <-->|semantic search| PolKB
    RegKB --- Foundry
    PolKB --- Foundry
```

---

## Component Descriptions

### Compliance Co-Pilot (Orchestrator)

The entry point for all user interactions. Its system instructions drive the full multi-step flow:

1. Call **Regulation IQ** for the regulatory stance (with clause citation).
2. Call **Internal Policy IQ** for the company stance (with doc citation).
3. Diff the two answers → produce a structured gap analysis: `topic`, `regulation_name`, `regulator_clause`, `internal_clause`, `gap_severity` (LOW / MED / HIGH), `recommendation`.
4. Render the **Gap Analysis Adaptive Card**.
5. If severity is MED or HIGH, offer to file a GitHub issue.
6. On user confirmation, call `issue_write` via the GitHub MCP server.

**Channels:** Teams, Microsoft 365 Copilot  
**Auth:** Integrated / Always (Microsoft Entra ID)  
**Schema name:** `cr47b_agent`

---

### Regulation IQ (Sub-Agent)

| Property | Value |
|---|---|
| Schema name | `new_RegulationIQ` |
| Knowledge source | Foundry IQ → SharePoint `Compliance/Regulations/` |
| Documents | `gdpr-summary.pdf`, `hipaa-privacy-rule.pdf`, `soc2-trust-criteria.pdf` |
| General knowledge | Disabled (`useModelKnowledge: false`) |
| Instruction | Quote exact article/section. Never speculate beyond cited text. |
| Hand-off trigger | _"Answers questions about external regulations (GDPR, HIPAA, SOC2). Always cites the exact article, section, and document name."_ |

---

### Internal Policy IQ (Sub-Agent)

| Property | Value |
|---|---|
| Schema name | `new_InternalPolicyIQ` |
| Knowledge source | Foundry IQ → SharePoint `Compliance/InternalPolicies/` |
| Documents | `data-handling-policy.docx`, `retention-standard.docx`, `access-control-baseline.docx` |
| General knowledge | Disabled (`useModelKnowledge: false`) |
| Instruction | Quote exact clause + document title. Never invent policy text. |
| Hand-off trigger | _"Answers questions about the company's internal policies. Always cites the exact document name, section number, and clause."_ |

---

### GitHub MCP Server

| Property | Value |
|---|---|
| Endpoint | `https://api.githubcopilot.com/mcp/` |
| Transport | Streamable HTTP |
| Authentication | OAuth 2.0 — user identity (Copilot Studio brokers the handshake) |
| Connection reference | `cr47b_agent.shared_github.a73a0ba95f4a47109aeb3c2e8bbd83a0` |
| Enabled tools | `issue_write`, `issue_read`, `list_issues`, `search_code`, `get_file_contents`, `search_issues`, `get_me`, `list_pull_requests`, `pull_request_read`, `search_pull_requests` |
| Token storage | Per-user, managed by Copilot Studio (silent on subsequent calls) |

---

### Gap Analysis Adaptive Card

Rendered inline in Copilot Chat. Contains:

- **Three-column table:** Regulator clause / Internal clause / Gap severity badge
- **Recommendation** text block
- **Action buttons:**
  - `📌 File GitHub Issue` → submits `create_issue` with `[SEVERITY]` prefixed title, both citations in body, labels `["compliance", severity]`
  - `✅ Mark as accepted risk` → closes the card flow

 The `adaptive-cards/gap-analysis-card.json` file in the repository documents the intended Adaptive Card layout .

---

## Data Flow — Detailed Sequence

```mermaid
sequenceDiagram
    actor User
    participant Orch as Compliance Co-Pilot
    participant RegA as Regulation IQ
    participant PolA as Internal Policy IQ
    participant Card as Adaptive Card
    participant GH as GitHub MCP

    User->>Orch: "Does our breach notification comply with GDPR?"
    Orch->>RegA: "What does GDPR say about breach notification?"
    RegA-->>Orch: "Art. 33: notify supervisory authority within 72 hours"
    Orch->>PolA: "What does our policy say about breach notification?"
    PolA-->>Orch: "Data Handling Policy POL-DATA-001 §4.3: notify within 5 business days"
    Orch->>Orch: diff() → gap_severity = HIGH
    Orch->>Card: render gap analysis card
    Card-->>User: displays Regulator / We say / HIGH gap
    User->>Card: clicks "File GitHub Issue"
    Card->>Orch: action=issue_write
    Orch->>GH: OAuth consent (first time only)
    GH-->>User: "Sign in to GitHub" popup
    User->>GH: authorizes
    Orch->>GH: issue_write(title="[HIGH] GDPR Art.33 breach notification gap", body=..., labels=["compliance","high"])
    GH-->>Orch: issue #42 URL
    Orch-->>User: "✅ Issue #42 opened → https://github.com/..."
```

---

## Intentional Compliance Gap (Demo Scenario)

The demo plants a deliberate mismatch in the knowledge bases:

| | GDPR Article 33 | Internal Policy §4.3 (POL-DATA-001) |
|---|---|---|
| **Breach notification deadline** | 72 hours | 5 business days |
| **Severity** | HIGH | — |
| **Recommendation** | Update policy to require 72-hour notification or shorter |

This gap is reliable, verifiable, and immediately understandable to any audience — making it ideal for a live demo.

---

## Security & Safety Controls

| Control | Implementation |
|---|---|
| Authentication | Microsoft Entra ID (Integrated/Always) |
| GitHub OAuth | User-identity flow; token stored per-user by Copilot Studio |
| Least-privilege | Only issue and search tools enabled; no repo admin or delete operations |
| Grounding | Foundry IQ with general knowledge disabled on both sub-agents |
| Content moderation | Copilot Studio content safety at High |
| No PII in issues | Orchestrator instruction explicitly prohibits including PII in filed issues |
