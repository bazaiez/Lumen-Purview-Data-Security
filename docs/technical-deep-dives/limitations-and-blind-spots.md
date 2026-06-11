# Limitations and Blind Spots

**Last Updated:** April 2026
**Status:** [VALIDATED]

---

## Overview

This document provides an honest assessment of Security Copilot's limitations when used with Purview Data Security. Understanding these constraints is essential for setting realistic expectations with customers and designing effective workflows.

## Data Access Limitations

### What SC Cannot See
- **File contents** — SC works with metadata, classifications, and policy match information, not raw file data
- **Custom SIT regex internals** — Built-in SITs are well-documented; custom SITs may produce opaque match explanations
- **Internal messaging or communication** — SC cannot read Teams/email message bodies for intent analysis
- **Endpoint activity outside DLP scope** — Activities not monitored by Purview DLP are invisible to SC

### Data Freshness Gaps
| Data Source | Typical Lag | Impact |
|-------------|------------|--------|
| DLP alerts | 15-30 minutes | Near real-time for triage |
| IRM alerts | Near real-time | Good for active threat detection |
| Audit logs | 24-48 hours | Timeline reconstruction may miss recent events (a Purview UAL platform characteristic, not SC-specific) |
| DSPM scans | Days to weeks (periodic) | Posture assessment reflects scan date, not current state |

## Analytical Limitations

### Intent and Context
- SC **cannot determine user intent** — it reports activities, not motivations
- **Business context is invisible** — SC doesn't know if a data transfer was manager-approved
- **Role-appropriate activity** is hard to distinguish from anomalous activity without organizational context
- **False positive assessment** requires human knowledge of legitimate workflows

### Correlation Constraints
- Cross-signal correlation (DLP + IRM) requires **data sharing to be enabled** between products
- SC correlates within Purview data sources only — external security tools require separate plugins
- Correlation does not imply causation — SC may surface patterns that aren't actually related

### Accuracy and Confidence
- SC does not provide confidence scores on its assessments
- Risk rankings are based on available signals — missing signals produce incomplete rankings
- "Summarize" prompts may omit details if the underlying data is complex
- Sequential prompts can drift from original context in long sessions

## Operational Limitations

### DLP Triage Agent (Microsoft Purview Triage Agent in DLP — GA)
- Only works with **active mode** DLP policies (not simulation/audit mode)
- Cannot process alerts from custom DLP solutions (only native Purview DLP)
- Triage categories are pre-defined — custom categorization requires manual setup
- Note: SC has no policy/alert write-back, but this agent can now send **Teams Remediation Reminders** (preview) — a Teams chat to a file's last-modified user asking them to remove sensitive content from SharePoint/OneDrive files triaged as "Needs attention". It still cannot close alerts or change policies.

### IRM Triage Agent (Microsoft Purview Triage Agent in Insider Risk Management — GA)
- Analyzed only SharePoint file content during preview (a preview-era residual; not email/device)
- Risk scoring is model-based — may not reflect actual insider threat risk in all contexts
- Role baselines may not exist for small teams or new employees
- Cannot assess organizational privilege abuse without RBAC context

### Data Security Posture Agent (Preview — serves both DSPM and DSI)
- The same agent delivers DSPM (natural-language data discovery) and Data Security Investigations (DSI, tenant-wide credential scanning) — it is not a separate DSI agent
- Natural language queries may return partial results for complex data estates
- Scan coverage depends on connectors configured — unscanned sources are invisible
- Cannot remediate findings directly — all actions are recommendations

### Promptbooks
- Context window limits apply — very long promptbook sequences may lose early context
- Parameter passing between steps works for simple values; complex objects may not pass cleanly
- No conditional branching in native promptbooks — decision logic requires human intervention between steps

## Deployment and Access Limitations

- **Cloud availability:** SC is available in commercial cloud. FedRAMP High commercial is GA. Not available in all sovereign clouds
- **Licensing:** Requires SCU capacity provisioned and Security Copilot access assigned
- **RBAC:** SC respects Purview RBAC — users only see data they have permission to access
- **Plugin state:** Purview plugin must be explicitly enabled per-tenant

## Workarounds and Mitigations

| Limitation | Workaround |
|-----------|-----------|
| Audit log lag | Supplement with near-real-time DLP/IRM alerts for urgent cases |
| No file content access | Use metadata, classification, and policy match details as proxy |
| Intent unknown | Combine SC output with manager interviews and HR context |
| No conditional promptbooks | Use playbook documents with decision trees; run steps manually |
| Long session drift | Start new sessions for distinct investigation phases |
| DSPM scan staleness | Note scan date in reports; schedule frequent scans for critical assets |

## Recommendations for CSAs

1. **Set expectations honestly** — SC accelerates investigation but does not replace analyst judgment
2. **Document blind spots per customer** — Each tenant has unique gaps based on configuration
3. **Test prompts in customer environment** — Prompt behavior varies by data volume and policy complexity
4. **Design human checkpoints** — Every workflow should have validation steps before action
5. **Track capability changes** — SC capabilities evolve; re-validate prompts quarterly

---

**See also:** [USAGE-GUIDE.md](../../USAGE-GUIDE.md) for the "Known Limitations and Gotchas" section with practical guidance.
