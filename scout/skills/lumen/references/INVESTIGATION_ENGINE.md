# Investigation Engine — playbook generator

Given an incident, produce a complete, human-followed investigation playbook with SC prompts at
every step, decision trees, remediation, and escalation. SC does not investigate — it equips a
human investigator. Never conclude guilt; present evidence for humans to decide.

## Step 1 — Classify the incident
| Type | Primary capabilities | Key audit operations |
|---|---|---|
| Data Exfiltration | Zoom Into User Risk, Get Data Risk Summary, Summarize Alert | Send, FileDownloaded, SharingSet, AnonymousLinkCreated, FileUploadedToCloud |
| Insider Threat | Get User Risk Summary, Zoom Into User Risk | file + email + sharing ops |
| DLP Policy Violation | Summarize Alert, Zoom Into Data Risk | DlpRuleMatch, policy-specific |
| Departing Employee | Get/Zoom User Risk | FileDownloaded, FileSyncDownloadedFull, FileCopiedToRemovableMedia |
| Unauthorized Sharing | Zoom Into Data Risk | SharingSet, SharingInvitationCreated, AnonymousLinkCreated, CompanyLinkCreated |
| Credential Exposure | **Data Security Posture Agent (in DSI)**, Get Data Risk Summary | FileUploaded, FileModified |
| Policy Gap | Triage Alerts, Zoom Into Data Risk | varies |
| Compliance Incident | all | all relevant |

## Step 2 — Initial severity
Score on: data sensitivity (PII/PHI/PCI volume), did data leave the org, correlated intent
signals, scope (users/depts), regulatory impact. Map to Critical/High/Medium/Low.

## Step 3 — Build the 5-phase playbook
- **Phase 1 — Initial Assessment (5–10 min):** alert summary + user/data risk overview.
  DECISION: real incident vs false positive (if FP → document + close).
- **Phase 2 — Deep Investigation (15–45 min):** user activity deep-dive (Operation Anchor with
  exact ops), data classification, timeline, DLP+IRM correlation. DECISION: severity → escalate/continue/monitor.
- **Phase 3 — Scope Assessment (10–20 min):** all affected data, all recipients/destinations,
  similar patterns across peers. DECISION: isolated vs broader pattern.
- **Phase 4 — Evidence Documentation (10–15 min):** investigation summary report, compliance
  doc if needed, evidence-preservation checklist.
- **Phase 5 — Remediation Recommendations:** immediate / short-term / long-term + escalation.

Each step: a copy-paste SC prompt + "what to look for" + which capability. Every activity step
uses exact audit-operation names ([AUDIT_LOG_OPERATIONS.md](AUDIT_LOG_OPERATIONS.md)).

## Remediation library (human actions — SC cannot execute these)
- **Immediate (0–4h):** revoke external sharing links; restrict/disable account (Entra);
  block external forwarding (Exchange); legal hold on files; revoke OAuth consents; notify IR.
- **Short-term (1–7d):** review/revoke external sharing for user; audit mailbox rules/delegates;
  reset credentials if compromise suspected; label unprotected sensitive files; expand DLP/IRM.
- **Long-term (1–4w):** tune DLP policies; conditional access for sensitive data; deploy
  endpoint DLP; awareness training; information barriers; auto-labeling.

## Escalation matrix
| Trigger | Escalate to | SLA |
|---|---|---|
| Confirmed PII/PHI exfiltration | Legal + Privacy Officer | 1h |
| Confirmed malicious insider | HR + Legal + CISO | 2h |
| Regulatory breach notification | Legal + Compliance + CISO | 4h |
| Multiple coordinated users | CISO + IR Lead | 2h |
| Credential compromise | SecOps + IT | 1h |
| Data to competitor/adversary | Legal + CISO + Exec | Immediate |

## Mandatory rules
1. Every prompt maps to one of the 6 capabilities. 2. Use Operation Anchor for activities.
3. Include decision points (never linear). 4. Remediation = human actions only.
5. Always: evidence preservation, escalation matrix, and a Limitations note (what SC could not
determine). 6. If it needs Defender/Sentinel/network forensics, say so explicitly. 7. Never
conclude guilt. 8. Give per-phase time estimates. Mark live incidents URGENT.

## Output skeleton
```
## Investigation Playbook: [title]
Incident Type / Initial Severity / Estimated Time / Required Roles / Prerequisites
### Phase N: [name] — Goal / Time
Step N.x: ```<SC prompt>``` — SC Capability / What to look for
DECISION POINT: if A → … / if B → …
### Remediation (Immediate/Short/Long) + Escalation
### Evidence Preservation Checklist
### Limitations
```
