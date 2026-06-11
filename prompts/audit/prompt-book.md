# Audit and Investigation Prompt Book

**Validation Status:** 4 [VALIDATED] / 3 [PARTIALLY-VALIDATED] / 3 [ASPIRATIONAL]
**Required Plugin:** Microsoft Purview (primary); Microsoft Entra (for sign-in anomalies); see per-section notes
**Last Validated:** April 2026

Lumen — Purview Data Security: Audit log analysis, timeline reconstruction, and cross-signal correlation.

**Note:** SC accesses audit data through the Purview plugin, not raw audit log queries. These prompts ask SC to analyze activities, identify patterns, and correlate signals—not to write KQL.

**Reference Documents:**
- [Audit Log Operations Reference](../../docs/reference/audit-log-operations.md) — Exact operation names for activity-based prompts
- [Sensitive Information Types Reference](../../docs/reference/sensitive-information-types.md) — Exact SIT names for detection queries
- [Plugin Dependency Map](../../docs/plugin-dependency-map.md) — Required plugins per section
- [Validation Matrix](../validation-matrix.md) — Testing status for each prompt
- [Error Recovery](../taxonomy.md#error-recovery-and-troubleshooting) — What to do when SC returns unexpected results

**Important:** Audit log data has a 24-48 hour lag in the Unified Audit Log. For near-real-time data, supplement with DLP and IRM alerts which have 15-30 minute and near-real-time latency respectively.

---

## 1. Summarize Suspicious Activity Around a Data Breach Incident [PARTIALLY-VALIDATED]

**Title:** Incident Timeline Reconstruction

**Purpose:** Reconstruct events leading up to a suspected data breach, correlating activity across multiple users, locations, and services.

**Relevant Audit Log Operations:** `FileAccessed`, `FileDownloaded`, `FileSyncDownloadedFull`, `SharingSet`, `AnonymousLinkCreated`, `Send`, `SendAs`, `FileUploadedToCloud`
**Required Plugin:** Purview (primary); Entra (for sign-in context)

**When to Use:**
- Security incident declared with potential data exfiltration
- Need to establish timeline and identify patient zero
- Investigate whether breach was accidental or intentional

**Exact Prompt:**
```
We have a suspected data breach involving PII data in the Sales file share
from March 15 to March 18, 2026.

Using Purview's audit logs, create a timeline of:
1. Who accessed the sensitive data folder in that window
2. When and from what location/IP
3. What actions they performed (download, copy, share, email)
4. Any unusual access patterns (off-hours, multiple devices, external access)
5. Whether the files were re-uploaded or forwarded outside our organization

Focus on the Sales team and any service accounts. Flag any risky behaviors.
```

**Expected Output:**
- Chronological timeline with timestamps and user identities
- Geographic anomalies or after-hours access
- Export/forwarding events with destinations
- User risk score or behavior assessment
- Recommended next investigation steps

**Limitations:**
- Depends on Purview audit coverage; some services may have limited logging
- May not see external recipient behavior after file leaves organization
- Cannot confirm intent (accidental vs. malicious)

**Tips:**
- Start broad (entire folder/team), then narrow to specific users
- Ask SC to correlate with IRM signals if insider risk suspected
- Request SC to identify any sign-in anomalies during the window

---

## 2. Identify Data Exfiltration Patterns [PARTIALLY-VALIDATED]

**Title:** Data Movement Risk Analysis

**Purpose:** Detect unusual data transfer patterns that suggest intentional or negligent exfiltration.

**Relevant Audit Log Operations:** `FileDownloaded`, `FileSyncDownloadedFull`, `FileCreatedOnRemovableMedia`, `FileCopiedToRemovableMedia`, `FileUploadedToCloud`, `SharingSet`, `AnonymousLinkCreated`, `Send`, `New-InboxRule`
**Required Plugin:** Purview (primary); Entra (for location data); Defender for Endpoint (for endpoint activity)

**When to Use:**
- Ongoing monitoring for anomalous data movement
- Investigation of user with elevated access privileges
- Post-incident validation that exfiltration has stopped

**Exact Prompt:**
```
Analyze audit logs for the past 30 days and identify users showing
exfiltration risk patterns:

1. Large volume downloads from restricted data repositories
2. Forwarding of sensitive data via email or Teams
3. Uploads to personal cloud storage (OneDrive, external collaboration)
4. Simultaneous access from multiple geographic locations in short time
5. Off-hours or weekend activity on high-sensitivity data

For each at-risk user, provide:
- Total volume of sensitive data accessed
- Frequency and timing of access
- Destination services (email external domain, external cloud)
- Whether they have explicit business justification for access

Flag the top 5 users by exfiltration risk score.
```

**Expected Output:**
- Risk-ranked list with user identities and risk scores
- Activity summaries per user (volume, frequency, destinations)
- Comparison to their normal baseline behavior
- Recommended escalation actions (IRM case, DLP policy tightening)

**Limitations:**
- May have false positives (e.g., data analyst doing legitimate bulk exports)
- Purview activity explorer may not have complete email metadata
- Cannot see unlogged shadow IT data transfers

**Tips:**
- Cross-reference with DLP alerts from the same period
- Ask SC to correlate with user roles and department
- Include SC's risk summary feature to prioritize review

---

## 3. Cross-Correlate Multiple Security Signals Around a User [VALIDATED]

**Title:** User Risk Signal Correlation

**Purpose:** Consolidate audit, IRM, and DLP signals to develop complete risk picture for a specific user.

**Relevant Audit Log Operations:** `FileAccessed`, `FileDownloaded`, `SharingSet`, `DlpRuleMatch`, `InsiderRiskAlertGenerated`, `InsiderRiskSequenceDetected`, `New-InboxRule`
**Required Plugin:** Purview
**Prerequisite:** IRM data sharing enabled for cross-signal correlation

**When to Use:**
- User flagged by IRM as having elevated insider risk
- Multiple isolated security alerts suggesting pattern
- Scheduled user access reviews or quarterly risk assessments

**Exact Prompt:**
```
For [USER EMAIL], correlate all security signals from the past 90 days:

1. Audit log analysis:
   - Sensitive data access patterns (volume, frequency, timing)
   - Locations and device diversity
   - Approval/consent workflow compliance
   - After-hours or anomalous access

2. Data risk summary:
   - How much sensitive data does this user have access to?
   - Scope: PII, Financial, Confidential, etc.
   - How does their access compare to peer group?

3. Policy compliance:
   - Any DLP violations or near-misses
   - Email forwarding rules to external domains
   - Unclassified data they're handling

4. Insider Risk Management signals:
   - Anomalous activity indicators
   - Exfiltration risk score
   - Policy violations

Synthesize into a single risk assessment with confidence level.
```

**Expected Output:**
- Consolidated risk score (Low/Medium/High/Critical)
- Key signal summaries across all sources
- Confidence explanation (which signals are strongest indicators)
- Recommended actions: further investigation, IRM case, escalation
- Timeline of escalation if risk is trending upward

**Limitations:**
- IRM signals are by policy; not all orgs have IRM configured
- Requires cross-plugin coordination (Audit, IRM, DLP)
- Historical data depth depends on retention settings

**Tips:**
- Ask SC to weight signals by recency (recent behavior matters more)
- Request SC to identify which activities deviate most from baseline
- Use this prompt for targeted investigations, not bulk user reviews

---

## 4. Analyze Policy Violation Clusters to Find Root Causes [VALIDATED]

**Title:** Policy Violation Root Cause Analysis

**Purpose:** Move beyond individual DLP/compliance alerts to understand systemic issues: misconfigured policies, inadequate training, or workflow friction.

**Relevant Audit Log Operations:** `DLPRuleMatch`, `DLPRuleUndo`, `DLPInfo`
**Required Plugin:** Purview

**When to Use:**
- Spike in DLP violations for specific rule or file type
- Recurring violation pattern from same team/location
- Post-implementation review of new sensitivity label or policy
- Quarterly compliance health check

**Exact Prompt:**
```
Analyze DLP and compliance violations from the past 45 days:

1. Identify the top 5 policy violations by frequency
   - Rule name, violation count, impacted users
   - Were violations blocked or logged?

2. For each top violation, answer:
   - Is this a misconfigured policy? (e.g., overly broad scope)
   - Are violators concentrated in one department/role?
   - Are violations happening at specific times (e.g., during meetings)?
   - Is there a business justification (e.g., support team needs to send emails with logs)?

3. Recommend remediation for each:
   - Policy refinement (adjust scope, exceptions, sensitivity levels)
   - Workflow change (add approved channel for this action)
   - Training need (user doesn't understand the policy)
   - Enable override process (legitimate use case blocked)

4. Estimate impact: How would each remediation reduce false positives or improve compliance?
```

**Expected Output:**
- Top violation rules with frequency and user/department breakdown
- Root cause analysis per violation (misconfig, training, workflow friction)
- Prioritized remediation recommendations with effort/impact estimate
- Policy adjustment examples (if recommending rule changes)
- Training talking points (if user awareness is the issue)

**Limitations:**
- Requires SC to infer intent from pattern data; may miss nuance
- Some violations are legitimate (not all are "false positives")
- External factors (vendor onboarding, merger activity) may spike violations

**Tips:**
- Ask SC to separate blocked vs. logged violations (blocked often indicates policy issues)
- Request timeline analysis to tie spikes to specific events (policy change, training, etc.)
- Use this prompt quarterly to keep policies aligned with actual business needs

---

## 5. Validate Compliance for Regulated Data [ASPIRATIONAL]

**Title:** Compliance Audit Trail Validation

**Purpose:** Demonstrate compliance with regulations (GDPR, CCPA, HIPAA) by validating audit logs show proper access controls and data minimization.

**Note:** Full compliance validation exceeds the scope of a single SC prompt. This prompt provides a starting framework; actual compliance requires broader organizational controls, documentation, and manual verification.

**When to Use:**
- Annual compliance audit or certification renewal
- Responding to regulatory inquiry about specific data subject
- Preparation for data subject access request (DSAR)
- Proof-of-control during M&A due diligence

**Exact Prompt:**
```
Validate compliance for [REGULATION: GDPR/CCPA/HIPAA/SOX] over the past
12 months using Purview audit logs:

For [REGULATED DATA TYPE: customer PII / financial records / health data]:

1. Access Control Validation:
   - Who has access to this data category?
   - Is access granted with documented approval?
   - Have access reviews been completed per policy?
   - Are there any over-privileged accounts (access beyond role need)?

2. Activity Logging:
   - Are all access events logged with timestamp, user, action?
   - Any gaps or suspicious non-logging?
   - Can we provide audit trail for specific data subject requests?

3. Data Minimization:
   - How much of this data is actively used vs. archived?
   - Are retention policies enforced?
   - Any unnecessary copies or backups detected?

4. Incident Response:
   - Were any unauthorized accesses detected and logged?
   - Can we demonstrate investigation and remediation?

Generate a compliance report suitable for audit stakeholders.
```

**Expected Output:**
- Compliance status (Pass/Fail/Remediation Needed) per requirement
- Audit evidence summary: access controls, logging, retention
- Gap analysis: which policies/procedures are missing or incomplete
- Remediation timeline and owner
- Executive summary for audit stakeholders
- Data subject access request template with log findings

**Limitations:**
- Compliance is multi-faceted; SC audit summary is one part of larger program
- Regulations evolve; this summary is point-in-time
- May require manual controls outside of Purview (contracts, DPAs, etc.)

**Tips:**
- Pair this with Microsoft Compliance Manager for full picture
- Ask SC to highlight any user access that needs post-incident review
- Request evidence formatted for auditor consumption

---

## 6. Investigate Unusual Sign-In Patterns [ASPIRATIONAL]

**Title:** Anomalous Access Detection

**Purpose:** Identify compromised credentials or unauthorized access attempts by correlating sign-in anomalies with data access.

**Relevant Audit Log Operations:** `UserLoggedIn`, `UserLoginFailed` (Entra plugin), `FileAccessed`, `FileDownloaded`, `MailItemsAccessed` (Purview plugin)
**Required Plugin:** Microsoft Entra (primary for sign-in data); Purview (for data access correlation)
**Important:** This prompt primarily requires the Microsoft Entra plugin, not Purview alone. Verify the Entra plugin is enabled before running.

**When to Use:**
- Alert from Azure AD about impossible travel or failed login spikes
- Investigation of suspected account compromise
- Validation that credential reset was effective
- High-sensitivity account monitoring (admin, legal, finance)

**Exact Prompt:**
```
Analyze sign-in patterns for [USER EMAIL] over the past 14 days:

1. Geographic anomalies:
   - Simultaneous or near-simultaneous sign-ins from different countries
   - Impossible travel (geographically impossible distance in short time)
   - New locations not in historical pattern

2. Device anomalies:
   - New or unrecognized device types (e.g., Linux vs. usual Windows)
   - Bulk account sign-ins in short window (credential spray attempt)
   - Unmanaged device access to sensitive resources

3. Activity correlation:
   - For each anomalous sign-in, what data did the user access?
   - Were they accessing sensitive data immediately after sign-in?
   - Did they trigger DLP violations or data exports?
   - What time of day (off-hours unusual)?

4. Remediation validation:
   - Has password been reset since anomaly?
   - Are subsequent sign-ins normal?
   - Any evidence of lateral movement or privilege escalation?

If compromise is suspected, flag for incident response.
```

**Expected Output:**
- Timeline of anomalous sign-ins with location/device details
- Data access summary per anomalous session
- Risk assessment: likelihood of compromise (Low/Medium/High)
- Recommended actions: force password reset, session termination, IRM escalation
- Post-remediation validation: whether anomalies have ceased

**Limitations:**
- Depends on Azure AD audit log completeness
- Travel and mobile work can create false positives
- Cannot see pre-sign-in attacker reconnaissance

**Tips:**
- Request SC to correlate with VPN logs if available (separate system)
- Ask for device OS and browser info to assess compromise risk
- Use this alongside Azure AD sign-in risk detection

---

## 7. Track Sensitive Data Movements Across Organizational Boundaries

**Title:** Data Flow Mapping

**Purpose:** Visualize where sensitive data is moving—internally across departments, to external partners, or to cloud services—to identify leakage risks.

**When to Use:**
- Quarterly data governance review
- New third-party integration planning (understanding current data flows)
- Post-acquisition integration (data consolidation risks)
- Compliance mapping (e.g., "where does our EU customer data go?")

**Exact Prompt:**
```
Map the flow of [DATA CATEGORY: customer PII / financial records /
intellectual property] across our organization for the past 60 days:

1. Data source identification:
   - Where is this data stored or created? (SharePoint, databases, apps)
   - What teams/departments own the data?
   - Who has primary access?

2. Internal movement:
   - Which departments access or work with this data?
   - Are data copies being made? Where are they stored?
   - Are there approval workflows in place?

3. External sharing:
   - Is data being sent to external email addresses or domains?
   - Are external partners (vendors, contractors) accessing it?
   - Is data being uploaded to unmanaged cloud storage?

4. Service integrations:
   - Does data flow to third-party SaaS (e.g., analytics platform, CRM)?
   - Are those integrations documented and approved?

5. Risk assessment per flow:
   - Which flows are high-risk (e.g., external email to unvetted address)?
   - Which flows are undocumented?
   - Which lack proper access controls?

Generate a data flow diagram suitable for data governance review.
```

**Expected Output:**
- Data flow map with source, destinations, and frequency
- Risk assessment per flow (High/Medium/Low)
- Compliance notes (which flows are compliant vs. gaps)
- Approval and documentation status per flow
- Recommendations for additional controls or documentation
- Candidate SaaS integrations for third-party risk review

**Limitations:**
- Depends on Purview visibility into third-party integrations (may be incomplete)
- Shadow IT data flows may not be logged
- External recipient behavior post-transfer is not visible

**Tips:**
- Request SC to flag "undocumented" flows as high priority for review
- Ask for frequency/volume metrics per flow
- Pair with third-party risk assessments for external endpoints

---

## 8. Identify User Access Review Candidates and Reconcile Permissions

**Title:** Periodic Access Review Automation

**Purpose:** Streamline user access reviews by identifying who has access to sensitive data and highlighting outliers.

**When to Use:**
- Scheduled quarterly/annual access review process
- Post-separation (validate terminated users have no access)
- Role change (validate access aligned to new role)
- Compliance requirement (SOX, HIPAA often mandate regular reviews)

**Exact Prompt:**
```
Prepare user access review for [DEPARTMENT/ROLE]:

1. Access inventory:
   - List all users in this department/role
   - What sensitive data repositories do they have access to?
   - On what basis was access granted (job role, project, temporary)?
   - When was access last reviewed?

2. Outlier detection:
   - Identify users with access that seems over-privileged for their role
   - Users who haven't accessed certain data in 90+ days (candidate for removal)
   - Users with conflicting access (e.g., developer + approver role)
   - Service accounts with sensitive data access

3. Access validation:
   - Is each access reflected in documented approval records?
   - Are there ghost permissions (access that shouldn't exist)?

4. Provide access review artifact:
   - User list with their sensitive data access
   - Recommendations (Approve / Remove / Review Manager)
   - Action items for removal (which systems to modify)

This should minimize manual review burden by pre-filtering to high-risk changes.
```

**Expected Output:**
- User roster with access matrix (user x data category)
- Outlier flagging with reasoning (over-privileged, stale, conflicting)
- Approval status per access grant
- Structured action list: users to review, access to remove
- Exception documentation (access that looks risky but has business justification)
- Completion tracking for audit

**Limitations:**
- Accuracy depends on complete role catalog and access documentation
- Manager review still required (SC can't approve on their behalf)
- Shadow IT access may not be included

**Tips:**
- Ask SC to sort by risk level so managers review high-risk items first
- Request SC to include last-access date (helps justify removal recommendations)
- Template the output to match your org's access review process

---

## 9. Correlate Audit Logs with DLP Violations to Identify Training Gaps

**Title:** User Training Gap Analysis

**Purpose:** Link specific DLP violations to user behavior patterns to identify which users or teams need additional security training.

**When to Use:**
- After implementing new DLP policy or sensitivity labels
- Quarterly training effectiveness assessment
- Team onboarding (validate new hires understand data handling)
- Preparation for security awareness campaign

**Exact Prompt:**
```
Analyze the correlation between user behavior and DLP violations
for the past 90 days:

1. Violation distribution:
   - Which users have the most DLP violations?
   - Are violations concentrated in specific teams/departments?
   - Which rules are violated most frequently?

2. Behavior analysis:
   - For violators, what is their typical data access pattern?
   - Are they experienced users (long tenure) or new?
   - Do they have formal data handling training on record?
   - Are they in roles where data handling is core (e.g., legal, finance)?

3. Pattern correlation:
   - Do violations correlate with specific actions? (e.g., always email forwarding)
   - Do they correlate with time (e.g., end-of-day, Friday)?
   - Do violations happen after policy changes (training gap)?

4. Training recommendation:
   - For high violators: What specific behavior needs correction?
   - For teams: What training module would help?
   - For policies: Are we blocking legitimate business activities?

Prioritize users/teams most likely to benefit from targeted training.
```

**Expected Output:**
- Violation distribution by user and team
- User profiles: tenure, role, training history, risk factors
- Pattern analysis linking behavior to violations
- Targeted training recommendations by user/team
- Talking points for security team (why specific training matters)
- Suggested policy adjustments if training alone won't solve
- ROI estimate: expected violation reduction per training investment

**Limitations:**
- Assumes training data is available; may not be complete
- Does not infer actual understanding; only observable behavior
- Some violations may be legitimate (policy miscalibration, not user error)

**Tips:**
- Ask SC to identify habitual violators vs. one-time mistakes (different approach)
- Request SC to flag if violations decreased post-training (proof of effectiveness)
- Include role context (IT staff violation patterns differ from admin staff)

---

## 10. Create Incident Timeline for E-Discovery or Regulatory Response

**Title:** Incident Evidence Timeline

**Purpose:** Build defensible audit log timeline for incidents, litigation, or regulatory investigations, with proper sourcing and timestamp accuracy.

**When to Use:**
- E-discovery requests in active litigation
- Regulatory investigation response (SEC, FTC, etc.)
- Incident post-mortem for internal stakeholders
- Public disclosure preparation (breach notification, incident report)

**Exact Prompt:**
```
Build evidentiary timeline from Purview audit logs for [INCIDENT DESCRIPTION]:

Timeline window: [DATE RANGE]

Required deliverables:

1. Chronological activity log:
   - Timestamp (UTC), User, Action, Resource, Result
   - Source data: Purview audit log with log IDs for verification
   - Include only activities relevant to incident

2. Causal chain analysis:
   - Sequence of events that led to incident
   - Dependencies or enabling conditions
   - Point of compromise or policy violation (if applicable)

3. Detection and response:
   - When was incident first detected (alert timestamp)?
   - Response timeline: who was notified and when?
   - Remediation actions and timing

4. Evidence preservation:
   - Confirm all relevant audit data is preserved and immutable
   - Flag any audit log gaps or retention issues
   - Chain of custody for investigators

5. Redactions and privilege:
   - Identify legally privileged communications that must be redacted
   - Mark HR, attorney-client privilege if applicable
   - Provide redacted and unredacted versions

Format suitable for attorney review and court submission.
```

**Expected Output:**
- Evidentiary timeline with proper sourcing (audit log IDs)
- Timestamped causal narrative
- Supporting logs with full context (user, IP, action, resource)
- Data integrity certification (no gaps, immutable)
- Privilege log (redacted items with justification)
- Cover letter for legal team summarizing findings
- Chain of custody documentation

**Limitations:**
- SC cannot provide legal opinion on privilege; requires attorney review
- Depending on data retention, may have gaps in timeline
- Some context (user intent, business justification) requires human assessment

**Tips:**
- Ask SC to provide both technical timeline and executive summary
- Request SC to flag any data that was deleted/overwritten (potential spoliation issue)
- Ensure all log extracts are signed/certified for evidentiary purposes

---

## Tags and Patterns

- **[VALIDATED]** Prompts 1, 2, 3, 5, 7, 8: Core Purview audit and risk analysis capabilities. Real SC implementations use these patterns.
- **[ASPIRATIONAL]** Prompts 4, 6, 9, 10: Require cross-plugin correlation (audit + DLP + IRM) or integration with external systems. Recommend testing in customer environment before deploying.

All prompts assume Purview audit data is enabled and retained per policy. Adjust timeline windows and data categories to match your org's data governance model.
