# Demo Script — Policy Compliance Co-Pilot

**Duration:** ~3 minutes  
**Surface:** M365 Copilot Chat (Teams or web)  
**Pre-req:** Agent published, GitHub repo `<yourname>/compliance-demo` exists, first-time GitHub OAuth done

---

## Scene 1 — The Hook (30s)

**Narrator:** "Every company has two sources of truth for compliance: what the regulator says, and what our own policy says. Today's demo asks: what happens when those two don't agree?"

**Type in Copilot Chat:**
> Does our data breach notification process comply with GDPR?

**Expected flow:**
1. Compliance Co-Pilot replies: "Analysing compliance... I'm checking both regulatory requirements and your internal policies."
2. _(Behind the scenes: calls Regulation IQ, then Internal Policy IQ)_
3. **Gap Analysis Adaptive Card** appears with:
   - 🔴 **HIGH** severity badge
   - **Regulator Says:** "GDPR Article 33: notify supervisory authority within **72 hours**"
   - **We Say:** "Data Handling Policy POL-DATA-001 §4.3: notify within **5 business days**"
   - **Gap:** "Company deadline (5 business days ≈ 120 hours) exceeds GDPR's 72-hour hard limit"
   - **Recommendation:** "Update §4.3 to require supervisory authority notification within 72 hours of confirming a breach"

---

## Scene 2 — One-Click GitHub Action (60s)

**Narrator:** "The gap is identified. Now the team needs a tracking item — without leaving the chat."

**Copilot says:** "This is a HIGH severity gap. Would you like me to file a GitHub issue to track remediation?"

**Type:**
> Yes, file it in myorg/compliance-demo

**Expected flow:**
1. Copilot asks for GitHub sign-in (first time only) — user clicks "Sign in to GitHub"
2. OAuth consent popup → user authorizes
3. Copilot: "Filing GitHub issue... ⏳"
4. Copilot: "✅ Issue #1 filed successfully! 🔗 https://github.com/myorg/compliance-demo/issues/1"

**Switch to GitHub tab** — show the issue:
- Title: `[HIGH] GDPR Art.33 breach notification gap`
- Body: full markdown with both citations, gap description, recommendation
- Labels: `compliance`, `high`

---

## Scene 3 — Second Query (60s)

**Narrator:** "Let's try another regulation."

**Type:**
> Is our data retention policy HIPAA-compliant?

**Expected flow:**
1. Adaptive Card appears — this time with a **LOW** or **MED** severity (HIPAA retention requirements vs. internal 7-year retention standard — likely aligned)
2. User clicks "✅ Mark as Accepted Risk" to close the card
3. Copilot acknowledges: "Noted. The risk has been accepted."

---

## Scene 4 — Wrap (30s)

**Narrator:** "No custom backend. No code deployed. Three Copilot Studio agents — one orchestrator, two Foundry IQ knowledge bases reasoning against each other — and a GitHub MCP server for write-back. The entire compliance workflow runs in Copilot Chat."

**Show the architecture diagram** from docs/ARCHITECTURE.md.

---

## Talking Points for Q&A

| Question | Answer |
|---|---|
| Why two sub-agents instead of one? | Separation of concerns: the regulation KB and policy KB stay independent, preventing cross-contamination of citations |
| Why GitHub MCP instead of a custom connector? | Remote MCP with user-identity OAuth means zero backend to maintain; the token is managed per-user by Copilot Studio |
| What about hallucination? | Both sub-agents have `useModelKnowledge: false` — they're grounded exclusively in Foundry IQ knowledge sources; if the document doesn't say it, the agent says so |
| How do we add more regulations? | Drop a PDF into the SharePoint `Compliance/Regulations/` folder — Foundry IQ indexes it automatically |
| Can this work for other compliance frameworks? | Yes — ISO 27001, PCI-DSS, FedRAMP; just add the PDFs to the regulation KB and update the orchestrator's instructions |
