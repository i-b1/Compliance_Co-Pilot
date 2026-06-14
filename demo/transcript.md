# Video Transcript — Policy Compliance Co-Pilot
**Target duration:** 90 seconds · **Track:** Agents League 2026 · Enterprise Agents

---

## [0:00 – 0:15] · ELEVATOR PITCH

> "Compliance teams spend hours cross-referencing what regulators require against what their own policies actually say — then more hours writing up the gaps and tracking remediation. What if that entire workflow happened in a single chat message?"

> "Meet the **Policy Compliance Co-Pilot**: ask a plain-English question in Microsoft Teams, and get a cited gap analysis between regulation and internal policy — with a one-click action to file a GitHub issue. No custom backend. No spreadsheets. Just chat."

---

## [0:15 – 0:35] · ARCHITECTURE

> "The system is three agents inside Microsoft Copilot Studio."

> "The **orchestrator** receives the user's question and delegates it in parallel to two specialised sub-agents, each powered by **Microsoft Foundry IQ**. **Regulation IQ** searches a SharePoint knowledge base of GDPR, HIPAA, and SOC2 documents and must cite the exact article. **Internal Policy IQ** does the same against our company policy library and must cite the exact clause."

> "General knowledge is disabled on both — they can only return what the documents actually say. The orchestrator then diffs the two answers, determines severity, and renders a **Gap Analysis Adaptive Card** inline in the chat. For medium or high severity gaps, the card offers a one-click action that calls the **GitHub MCP server** over OAuth to file a tracked issue — all without leaving the conversation."

---

## [0:35 – 1:10] · DEMO

> "Let's ask: *Does our data breach notification process comply with GDPR?*"

> _(Copilot Chat — type the question)_

> "The orchestrator calls Regulation IQ first — it returns GDPR Article 33: *notify the supervisory authority within 72 hours*. Then Internal Policy IQ — it finds our Data Handling Policy POL-DATA-001, section 4.3: *notify within 5 business days*."

> _(Gap Analysis Adaptive Card appears)_

> "The card surfaces the gap immediately: our policy allows up to 120 hours — that's 48 hours over the legal limit. Severity: **HIGH**."

> "I click **File GitHub Issue** — Copilot brokers the GitHub OAuth handshake, and within seconds:"

> _(Switch to GitHub tab)_

> "Issue filed. Title prefixed `[HIGH]`, body includes both citations and the recommendation, labels set to `compliance` and `high`. The audit trail is in GitHub before the conversation ends."

---

## [1:10 – 1:25] · CONTEST CRITERIA

> "Every judging criterion is addressed. **Multi-step reasoning**: five-stage orchestration — triage, two independent sub-agent calls, structured diff, and conditional GitHub action. **Accuracy**: Foundry IQ with grounding-only mode — every claim is sourced or withheld. **Reliability**: Microsoft Entra ID authentication, OAuth user-identity for GitHub, and least-privilege MCP — only five tools enabled. And all four enterprise bonus criteria: connected agents, external MCP server, OAuth security, and Adaptive Cards — that's the full thirty-three bonus points."

---

## [1:25 – 1:30] · CLOSE

> "Two AI agents that disagree by design. One that turns the gap into a GitHub issue. Zero custom infrastructure. **Policy Compliance Co-Pilot.**"

---

*On-screen at close: architecture diagram + badge row — Connected Agents · Foundry IQ · GitHub MCP OAuth · Adaptive Card · No Custom Backend*
