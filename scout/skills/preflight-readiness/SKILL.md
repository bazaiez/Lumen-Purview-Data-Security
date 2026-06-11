---
name: preflight-readiness
description: >
  Verify that a tenant/customer is ready to use Security Copilot with Microsoft Purview BEFORE a
  demo, pilot, or deployment. Produces a prerequisite checklist and a go/no-go readiness
  assessment across licensing, SCU capacity, plugin enablement, RBAC, policy mode, agent setup,
  and cloud availability. Use whenever the user asks "are we ready for…", "what do I need before…",
  "prerequisites for Security Copilot/Purview", "preflight", "readiness check", "before the
  customer demo/pilot", or is planning a Purview data-security engagement. Pairs with the
  lumen skill.
---

# Preflight Readiness

Check prerequisites and produce a go/no-go before a Security Copilot + Purview engagement.
Catch the blockers that quietly break demos: simulation-mode DLP policies, missing SCU
capacity, plugin not enabled, GCC cloud, or thin IRM baselines.

## Workflow
1. **Pick the engagement type** — demo / pilot / production deployment (deeper checks downstream).
2. **Walk the checklist** in [READINESS_CHECKLIST.md](references/READINESS_CHECKLIST.md), recording
   Pass / Fail / Unknown for each item with a short note.
3. **If the user is present and authorizes it**, you may help confirm tenant facts (e.g., open
   the Security Copilot / Purview admin pages in the browser, or use M365 context). Never change
   configuration — read-only verification only.
4. **Produce the readiness report** (format below) with an overall **GO / GO-WITH-RISKS / NO-GO**.
5. Offer to export it via the report-deliverable-generator skill.

## Critical blockers (NO-GO if failing)
- No Security Copilot access / no SCU capacity provisioned (note: M365 E5 now includes default
  SCU capacity — confirm it's active).
- Purview plugin not enabled in the SC admin center.
- Tenant on GCC / sovereign cloud (SC commercial only; FedRAMP High GA on Azure Commercial).
- For agent demos: DLP policies in **simulation mode** (DLP Triage Agent needs **active mode**).

## High-risk items (GO-WITH-RISKS)
- No recent DSPM scan (Posture Agent results will be stale or empty) — Posture Agent is preview.
- IRM Triage Agent expected to cover email/device — it analyzes **SharePoint file content only**.
- Cross-signal (DLP+IRM) correlation expected but **data sharing not enabled**.
- Thin IRM baselines (small teams / new hires) → noisy or sparse risk scores.
- Few/no alerts in the last 30 days → agents have little to triage (non-rolling 30-day window).
- Agents running under personal identity instead of a **Microsoft Entra Agent ID**.

## Output
```
## Security Copilot + Purview — Readiness Report
Engagement: demo | pilot | deployment        Overall: GO | GO-WITH-RISKS | NO-GO
### Checklist (table: Item | Status | Note)
### Blockers (must resolve)
### Risks (mitigations)
### Recommended actions before the date
```

## References
- [READINESS_CHECKLIST.md](references/READINESS_CHECKLIST.md) — full itemized checklist with how-to-verify.

## Post-Run Reflection
Did I separate hard blockers from risks? Did I keep verification read-only? Did I reflect the
correct GA/preview + active-mode + 30-day-window facts? Fix before delivering.
