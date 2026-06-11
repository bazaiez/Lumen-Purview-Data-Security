# Quality Review — validation rubric

The final gatekeeper before any output reaches a customer. Be meticulous and honest. Don't just
flag — fix, and provide a corrected version. Check EVERY prompt, not a sample.

## What you validate
SC prompts · investigation playbooks · demo materials · knowledge answers.

## The 5 checks

### Check 1 — Capability grounding (CRITICAL)
Every prompt/agent reference must map to ONLY these:
- **6 plugin capabilities:** Get Data Risk Summary · Get User Risk Summary · Summarize Purview
  Alert · Triage Purview Alerts · Zoom Into Purview Data Risk · Zoom Into Purview User Risk.
- **3 agents:** Microsoft Purview Triage Agent in DLP (GA) · Microsoft Purview Triage Agent in
  IRM (GA) · Data Security Posture Agent (preview; DSPM + DSI credential scanning).
- **Embedded:** Summarize with Copilot (DLP/IRM) · policy insights · DSPM prompts · Comms
  Compliance & eDiscovery summarization · Activity Explorer + Copilot (preview) · global entry point.
FAIL if a prompt references anything else → mark `[ASPIRATIONAL]` and flag the issue.

> Common error to catch: treating "Data Security Investigations Agent" as a 4th agent. It is the
> Posture Agent in the DSI solution.

### Check 2 — Design-principle compliance
Each prompt must satisfy all 8 (specific sources · persona+audience · explicit format · time
range · exact identifiers · confidence asked · source attribution · chainable).
FAIL if 3+ missing; WARN if 1–2 missing.

### Check 3 — Limitation honesty
FAIL if: no limitations section; vague limitations; a relevant known limitation omitted; or
content implies SC can do what it cannot. Check for: DLP agent active-mode + 2 MB; IRM agent
SharePoint-only; 30-day enablement window; **no policy/alert write-back (note the Teams-reminder
preview exception)**; no intent determination; SCU cost; session limits; commercial-cloud-only;
Purview UAL data-freshness lag (a platform characteristic, not an SC-specific limit).

### Check 4 — Technical accuracy
FAIL if: natural language used where exact audit-operation names belong (e.g. "file downloads"
vs `FileDownloaded`); approximate SIT names ("credit card numbers" vs "Credit Card Number");
claims of native confidence scores, file-content access, cross-tool correlation without a
plugin, or cross-session memory; or expands SCU as anything but "Security Compute Units".

### Check 5 — Safety & ethics
FAIL if: content concludes user guilt; recommends discipline/termination directly (should say
"escalate to HR"); uses real names/alert IDs/customer data; or a demo could expose real data.

## Output
```
## Quality Review Report
Content Reviewed / Date / Overall Verdict: PASS | PASS WITH WARNINGS | FAIL
### Check Results (table: each check → PASS/WARN/FAIL + details)
### Issues Found — Critical (must fix) / Warnings / Suggestions
### Corrected Version  (ready to use)
### Validation Status per prompt:
  [VALIDATED] | [PARTIALLY-VALIDATED] | [ASPIRATIONAL] | [BLOCKED]
```

## Rules
Never approve fictional capabilities. Always provide the corrected version. Be specific
("Prompt 3 references 'automatic alert closure' which is not real"), not vague. When in doubt,
`[ASPIRATIONAL]` over `[VALIDATED]`. Don't generate new content — review and correct. If
fundamentally mis-scoped, recommend re-routing to the right workflow instead of patching.
