# Prompt Taxonomy: Lumen — Purview Data Security

A framework for classifying, designing, and evaluating prompts used with Microsoft Security Copilot to investigate and respond to incidents within Microsoft Purview Data Security.

---

## 1. Prompt Classification System

Prompts are organized into nine categories based on their primary investigative or operational function. Each category addresses a distinct phase in the incident lifecycle or a specific analytical task.

### Triage Prompts

**Purpose**: Rapidly assess alert severity, prioritize incidents for investigation, and route to appropriate responders.

**When to Use**:
- A new alert has arrived and you need to decide whether to investigate or close it
- Multiple incidents are queued and you need to prioritize
- You need a quick severity score to gate downstream investigation

**Output**: Severity rating (critical/high/medium/low), recommended action (immediate investigation / schedule / close), suggested responder role

**Example Purview Context**:
- DLP policy match alert: Assess risk based on content type, recipient domain, policy violation history
- Insider risk activity: Evaluate risk level based on activity type, user tenure, historical patterns
- Information barrier violation: Determine severity based on sensitivity group definitions and communication scope

**Key Characteristics**:
- Fast turnaround (seconds to minutes)
- High false-positive tolerance (erring toward investigation)
- Simple decision criteria
- Outputs feed investigation prompts

---

### Investigation Prompts

**Purpose**: Conduct deep-dive analysis of an incident, correlate indicators, reconstruct timelines, and uncover root cause.

**When to Use**:
- You have triaged an alert as worthy of investigation
- You need to understand how an incident unfolded
- You need to identify all affected entities (users, workloads, data)
- You need to correlate signals from multiple sources to confirm the incident

**Output**: Detailed timeline, correlated indicators, affected entities, evidence chain, root cause hypothesis, confidence level

**Example Purview Context**:
- Reconstruct a multi-step data exfiltration: Query DLP policy matches, mail delivery rules, shared mailbox access logs, and federated sharing events
- Investigate suspicious insider activity: Correlate content access logs, downloads, sharing, and external communication
- Trace an information barrier violation: Map communication events across Exchange, Teams, and file sharing

**Key Characteristics**:
- Deeper data exploration, multiple correlated sources
- Outputs include evidence and source attribution
- High confidence requirement
- Often chained: one investigation prompt output informs the next

---

### Summarization Prompts

**Purpose**: Condense investigation findings and translate technical details for non-expert audiences.

**When to Use**:
- You need to brief a manager or executive on incident findings
- You need to explain technical findings to a non-security stakeholder
- You need a one-page summary of a complex investigation
- You need to identify the key takeaway or business impact

**Output**: Concise summary in plain language, business impact statement, key findings ranked by importance, technical detail level matched to audience

**Example Purview Context**:
- Executive summary of a DLP incident: "X users attempted to share Y sensitive files externally. The policies blocked Z attempts. No data left your organization."
- Non-technical explanation of insider risk activity: "User accessed more files than usual and downloaded sensitive data to an external device."
- Information barrier violation impact: "Two communication groups that should be separate exchanged information. 45 users were exposed."

**Key Characteristics**:
- Audience-aware (analyst, manager, executive, compliance officer)
- Avoids jargon or defines it
- Business impact focused
- Typically derived from investigation output

---

### Reporting Prompts

**Purpose**: Generate formal incident reports, compliance documentation, and post-incident analysis for audit and governance.

**When to Use**:
- An incident requires formal documentation for compliance or legal hold
- You need to generate a post-incident review for stakeholders
- You need to report the incident to regulators or external parties
- You need to update an IR ticket or SOAR record with structured findings

**Output**: Formatted report with incident metadata, timeline, impact assessment, remediation steps, lessons learned, compliance implications

**Example Purview Context**:
- DLP incident report: Policy name, violation count, affected departments, data sensitivity, whether data exfiltrated, policy tuning recommendations
- Insider risk post-mortem: User profile, activity types, organizational access, whether access was revoked, policy or training recommendations
- Information barrier incident documentation: Entities involved, communication details, whether isolation was broken, control recommendations

**Key Characteristics**:
- Formal structure (sections, metadata, signatures)
- Audit-ready language and sourcing
- Often includes remediation and prevention steps
- Typically derived from investigation or summarization output

---

### Enrichment Prompts

**Purpose**: Add contextual information to raw alert data so analysts can make better decisions without manual lookups.

**When to Use**:
- An alert lacks context about the user, workload, or data involved
- You need to understand whether an activity is expected or anomalous
- You need to correlate an alert with the user's past behavior or organizational role
- You need to identify which policies or risk factors are relevant to an incident

**Output**: Enriched alert with user context (role, tenure, historical behavior), data context (sensitivity, previous violations, affected departments), policy context (related policies, tuning history)

**Example Purview Context**:
- User context enrichment: Job title, reporting chain, department, location, risk level, previous policy violations, typical file sharing patterns
- Data context enrichment: Sensitivity labels, relevant DLP policies, content classification, previous matches, affected departments
- Incident context enrichment: Similar past incidents, policy tuning history, user's previous involvement, organizational risk factors

**Key Characteristics**:
- Pulls information from multiple Purview sources and external HR/directory
- Identifies patterns and anomalies
- Often a preprocessing step before investigation or hunting
- Inputs inform severity assessment and investigation direction

---

### Hunting Prompts

**Purpose**: Proactively search for indicators of compromise, policy violations, or anomalous behavior across your environment.

**When to Use**:
- You want to detect incidents before alerts fire
- You are investigating an incident and need to find related activity
- You need to assess whether a particular behavior is widespread
- You are looking for a specific data type or activity across all users and workloads

**Output**: Ranked list of findings (entities, activities, confidence), frequency and distribution data, correlation with known threats or patterns, recommended next steps

**Example Purview Context**:
- Hunt for bulk data transfers by looking for users with unusually high download counts, especially to external or personal devices
- Hunt for policy violations from a specific department: Search all DLP matches from finance team in the last month
- Hunt for suspicious sharing: Identify users sharing sensitive documents externally across multiple policies
- Hunt for information barrier violations: Search for communication across sensitive group boundaries

**Key Characteristics**:
- Broad scope (all users, workloads, or time range)
- Ranked output (confidence, frequency, risk)
- Often results in multiple new investigations
- Proactive rather than reactive

---

### Advisory Prompts

**Purpose**: Generate recommendations for policy tuning, risk mitigation, and security improvements based on incident history or environment state.

**When to Use**:
- An incident reveals a gap or misconfiguration in existing policies
- You need recommendations for hardening a specific data type or workload
- You need to propose new DLP policies or insider risk settings
- You need to suggest organizational or technical controls

**Output**: Ranked recommendations with justification, estimated impact, implementation effort, policy examples or templates, success metrics

**Example Purview Context**:
- DLP policy tuning: "Based on X false positives, consider widening the scope of policy Y or tuning the sensitive information type"
- Insider risk settings: "This user's activity pattern suggests role-based access controls could reduce risk with minimal impact on productivity"
- Information barrier improvements: "Consider automating group membership updates to reduce manual barrier management"
- Sharing policy hardening: "External sharing is restricted to approved domains, but current allow list includes X high-risk domains"

**Key Characteristics**:
- Grounded in observed incidents or environment state
- Practical and implementable
- Includes trade-offs (security vs. usability)
- Provides templates or examples

---

### Operational Prompts

**Purpose**: Assess the readiness, health, and configuration of Purview Data Security controls and investigate operational issues.

**When to Use**:
- You need to verify that Purview is working correctly
- You need to audit policy configurations or assignments
- You need to confirm that all workloads and users are monitored
- You need to troubleshoot missed alerts or false negatives

**Output**: Configuration audit results, health status, coverage gaps, misconfigurations, remediation steps

**Example Purview Context**:
- DLP policy audit: Which policies are assigned? Which locations are monitored? Are rule conditions correct?
- Insider risk coverage: Are all users in scope? Are risk indicators configured? Is the ML model updated?
- Information barrier health: Are groups correctly defined? Are barriers enforced? Are there orphaned or misconfigured barriers?
- Audit log coverage: Are Purview events being logged? Are there gaps in monitoring?

**Key Characteristics**:
- Focuses on control configuration and technical state
- Identifies gaps or misconfigurations
- Often triggered by missed detections or operational questions
- Results in config changes or escalations

---

### Workshop / Demo Prompts

**Purpose**: Demonstrate Security Copilot capabilities to customers, partners, or new team members in educational or hands-on scenarios.

**When to Use**:
- You are conducting a customer demo
- You are training new analysts
- You need to show a specific investigative workflow
- You want to showcase integration between Purview and Copilot

**Output**: Detailed walkthrough with realistic incident data, explanation of how Copilot speeds investigation, sample findings, recommended next steps

**Example Purview Context**:
- "Investigate a simulated phishing campaign that involved shared documents with malware." (Uses realistic sample data)
- "A user resigned and their files were shared externally. Investigate the timeline and impact." (Workflow walkthrough)
- "Design a DLP policy to protect credit card data. Use Copilot to predict false positives." (Capability showcase)

**Key Characteristics**:
- Uses realistic but safe sample data
- Highly narrated and educational
- Often includes expected output and talking points
- Good for onboarding and capability communication

---

## 2. Prompt Patterns

Reusable patterns reduce cognitive load and make prompts more discoverable. Each pattern has a standard structure and multiple instantiations across Purview use cases.

### Pattern 1: The Summarizer

**Template**:
```
Summarize [data type] from [Purview source] for [audience]
focusing on [aspect].
Include [sections].
```

**When to Use**:
- You have detailed investigation findings and need a concise version
- You need to brief stakeholders at different levels of detail
- You need to create a finding for your SOAR system

**Example with Purview**:
```
Summarize the DLP policy violations from the last 7 days
for our CISO, focusing on data exfiltration attempts.
Include: total violations, affected departments, severity breakdown,
and whether any data left the organization.
```

**Tips**:
- Name a specific audience so Copilot can adjust terminology
- Specify the aspect to avoid generic summaries
- Request a bulleted or prose format depending on your audience

---

### Pattern 2: The Investigator

**Template**:
```
Investigate [incident/alert] by analyzing [Purview data sources].
Look for [specific indicators].
Provide [output format].
Include confidence levels and data sources.
```

**When to Use**:
- You have a triaged alert and need to understand it deeply
- You need to correlate multiple signals to confirm an incident
- You need a timeline or evidence chain

**Example with Purview**:
```
Investigate the user's attempt to share sensitive files externally
by analyzing DLP policy matches, mail rules, federated sharing events, and recent device access.
Look for exfiltration patterns, unusual timing, or coordination with external recipients.
Provide a timeline of events with confidence levels and data sources.
```

**Tips**:
- Name multiple data sources (DLP, mail, sharing, audit logs)
- Be specific about indicators you're looking for (not just "suspicious activity")
- Always ask for confidence levels and source attribution

---

### Pattern 3: The Triager

**Template**:
```
Given [alert details], assess severity based on [criteria].
Recommend [action type].
Include your scoring rationale.
```

**When to Use**:
- A new alert arrives and you need a quick severity decision
- You need to prioritize multiple alerts
- You need to gate investigation with a fast initial assessment

**Example with Purview**:
```
Given a DLP policy match for a confidential document shared
with a personal Gmail account, assess severity based on:
- whether the document left the organization
- the number of previous matches from this user
- the data classification and affected department

Recommend whether this requires immediate investigation or can be queued.
Include your scoring rationale.
```

**Tips**:
- Reference specific alert metadata (user, policy, content type)
- List 3-4 severity criteria to avoid vague assessments
- Request a clear recommendation (investigate now, queue, close)

---

### Pattern 4: The Explainer

**Template**:
```
Explain [technical finding or alert] in terms a [persona] would understand.
Include [specific context].
Avoid [jargon] or define it.
```

**When to Use**:
- You need to brief a non-security stakeholder (manager, legal, compliance)
- You need to explain a false positive to a user
- You need to communicate risk in a way that drives action

**Example with Purview**:
```
Explain the DLP policy violation to the user's manager
in terms a non-technical leader would understand.
Include: what the user did, why it triggered a policy, what data was involved,
and what the business impact could have been.
Avoid technical terms like "sensitive information type" or define them briefly.
```

**Tips**:
- Always name the audience (manager, executive, compliance officer)
- Focus on business impact, not technical details
- Use concrete examples, not abstract risk language

---

### Pattern 5: The Reporter

**Template**:
```
Generate a [report type] covering [scope] for [audience].
Include [sections].
Format for [output medium, e.g., email, compliance system, PDF].
```

**When to Use**:
- An incident requires formal documentation
- You need to create an auditable record
- You need to report to regulators or external parties

**Example with Purview**:
```
Generate an incident report covering the DLP violations
from the last 30 days for our compliance team.
Include: violation count, affected data types, affected departments,
remediation actions taken, and policy tuning recommendations.
Format for our compliance system (CSV with standard fields).
```

**Tips**:
- Name the report type (incident report, post-mortem, trend report)
- Specify exact sections you need for your downstream system
- Request a format that integrates with your workflow

---

### Pattern 6: The Advisor

**Template**:
```
Based on [current state], recommend [improvement type]
considering [constraints].
Include implementation effort and success metrics.
```

**When to Use**:
- An incident reveals a policy gap or misconfiguration
- You need recommendations to harden a control
- You need to propose new policies or settings

**Example with Purview**:
```
Based on the high number of DLP false positives from our HR department,
recommend improvements to our sensitive information type definitions
considering the need to protect actual SSNs without blocking legitimate HR communications.
Include implementation effort and metrics to measure success.
```

**Tips**:
- Ground recommendations in observed incidents or data
- Name constraints (business need, usability, organizational risk tolerance)
- Provide metrics to measure whether the recommendation worked

---

### Pattern 7: The Hunter

**Template**:
```
Search for [indicator/pattern] across [Purview sources]
from [time range].
Flag [criteria] and rank by [ranking method].
Include frequency distribution and confidence.
```

**When to Use**:
- You want to proactively detect incidents
- You need to find related activity during an investigation
- You need to assess whether a behavior is widespread

**Example with Purview**:
```
Search for bulk downloads to external or personal devices
across audit logs and DLP data from the last 90 days.
Flag users with download activity 3 standard deviations above their peer group.
Include frequency distribution and confidence levels.
```

**Tips**:
- Define the indicator or pattern precisely (not just "suspicious activity")
- Specify time range to focus the hunt
- Request ranking and distribution so you can prioritize investigation

---

### Pattern 8: The Correlator

**Template**:
```
Correlate [signal A] with [signal B] for [entity]
from [time range].
Identify [patterns to flag] and assess [question].
Provide confidence and alternative explanations.
```

**When to Use**:
- You need to confirm a suspected incident by showing multiple signals align
- You need to rule out false positives by checking for expected signals
- You need to identify sophisticated attacks that span multiple data sources

**Example with Purview**:
```
Correlate DLP policy matches with insider risk activity
for this user from the last 30 days.
Identify patterns where the user accessed sensitive files,
then shared them externally, then deleted evidence.
Provide confidence levels and alternative explanations for each signal.
```

**Tips**:
- Identify at least 2 signals that should correlate
- Be specific about the pattern you're looking for (sequencing, timing, entities)
- Always ask for alternative explanations to avoid premature conclusions

---

## 3. Prompt Design Principles for Security Copilot

All prompts in this library follow these principles to maximize quality, auditability, and usefulness.

### Principle 1: Be Specific About Purview Data Sources

**Don't**: "Investigate the user's activity."
**Do**: "Investigate the user's activity by analyzing DLP policy matches, mail delivery rules, shared mailbox access logs, and federated sharing audit events."

**Why**: Copilot performs better when you name the specific Purview sources you want analyzed. Generic requests often return generic results.

**How**: Reference actual Purview data sources:
- DLP policy matches and rule matches
- Insider risk activity indicators (file access, downloads, copying, sharing, etc.)
- Information barrier communication events
- Mail flow rules and external sharing logs
- OneDrive and SharePoint audit logs
- Exchange mailbox access and delegation
- Teams message analysis

---

### Principle 2: Name the Persona and Audience for Output

**Don't**: "Summarize the findings."
**Do**: "Summarize the findings for our CISO, who cares about business impact and whether we need to notify customers."

**Why**: Copilot adjusts tone, detail level, and terminology based on audience. A CISO summary looks different from an analyst summary.

**How**: Identify your audience:
- Incident responder (technical, wants evidence and timeline)
- Security analyst (technical, wants patterns and next steps)
- Manager (non-technical, wants impact and status)
- CISO (business-focused, wants risk and compliance implications)
- Compliance officer (audit-focused, wants controls and evidence trail)
- General user (non-technical, wants explanation of what happened)

---

### Principle 3: Specify Output Format Explicitly

**Don't**: "List the violations."
**Do**: "List the violations in a ranked table: user | policy name | content type | recipient domain | severity | evidence."

**Why**: Explicit format requests reduce iteration and make outputs directly usable in your workflow.

**How**: Choose your format:
- **Timeline**: Chronological sequence of events with timestamps
- **Table**: Ranked or categorized rows for easy parsing
- **Bullet points**: Concise list of findings
- **Narrative**: Prose explanation for non-technical readers
- **Ranked list**: Ordered by severity, confidence, or frequency
- **Evidence chain**: Events linked causally with supporting data

---

### Principle 4: Include Time Ranges

**Don't**: "Find all insider risk activity from this user."
**Do**: "Find all insider risk activity from this user in the last 30 days."

**Why**: Time bounds focus analysis and prevent historical noise from swamping recent findings.

**How**: Specify time ranges:
- Incident-centric: "from the alert timestamp ±1 week"
- Period-centric: "in the last 30 days", "from 2026-03-01 to 2026-03-31"
- Relative: "since their hire date", "in their last 90 days of employment"
- Comparative: "compared to their behavior 6 months ago"

---

### Principle 5: Reference Specific Policy Names, Alert Types, or User Identifiers

**Don't**: "Check the DLP policy for this user."
**Do**: "Check the 'Confidential Financial Data' DLP policy matches for alice@contoso.com in the last 7 days."

**Why**: Specific identifiers ground analysis in your actual environment and avoid confusion with similar policies or users.

**How**: Always include:
- Full policy names (not abbreviated)
- User email addresses or identifiers
- Specific alert types (not "suspicious activity")
- Data classification labels you use
- Organizational units or departments
- External domains you're monitoring

---

### Principle 6: Ask for Confidence Levels and Caveats

**Don't**: "Is this user exfiltrating data?"
**Do**: "Based on the indicators you found, is this user likely exfiltrating data? Provide a confidence level and explain what would make you more or less confident."

**Why**: Security findings are rarely 100% certain. Requesting confidence levels forces explicit reasoning and prevents overconfident conclusions.

**How**: Include in every investigation or hunting prompt:
- "Confidence level for each finding (high/medium/low)"
- "What data sources would increase your confidence?"
- "What alternative explanations exist?"
- "What are you not seeing that would be expected?"

---

### Principle 7: Request Source Attribution

**Don't**: "Summarize what happened."
**Do**: "Summarize what happened and cite your data sources (policy name, log type, user count) so I can verify findings."

**Why**: Auditable findings require source attribution. It also allows you to discover gaps in monitoring or data quality.

**How**: Always request:
- Which Purview data source each finding comes from
- Specific policy names, log types, or event IDs
- Whether data is directly observed or inferred
- Whether any findings depend on external data (HR, threat intel)

---

### Principle 8: Design Prompts to Chain

**Single Prompt**: "Investigate this DLP alert and generate a report."
**Chained Prompts**:
1. Triage prompt → severity score
2. Investigation prompt → detailed timeline and evidence
3. Enrichment prompt → context about user and data
4. Summarization prompt → one-pager for manager
5. Reporting prompt → audit-ready incident report

**Why**: Complex investigations often require multiple perspectives. Chaining allows each prompt to focus on one job, improving quality and auditability.

**How**: Design prompts so outputs feed into downstream prompts:
- Triage output includes entity identifiers and severity factors that investigation prompt uses
- Investigation output is structured (timeline, entities, evidence) so enrichment can add context
- Enrichment output becomes context for summarization and advisory prompts

**Example chain**:
```
Step 1 (Triage): Assess alert severity
→ Output: "High severity, investigate immediately, user has prior violations"

Step 2 (Investigation): Deep-dive using triage output
→ Output: "Timeline of events, 5 affected files, 3 external recipients, no evidence of exfiltration"

Step 3 (Enrichment): Add context using investigation output
→ Output: "User is junior analyst with known access to these files, external recipients are consulting partners, prior approvals exist"

Step 4 (Summarization): Simplify for manager
→ Output: "Likely false positive, user legitimately shared approved data with external consulting partner"

Step 5 (Advisory): Recommend improvements
→ Output: "Consider adding consulting partner domain to policy whitelist, monitor this user's sharing patterns"
```

---

## 4. Prompt Quality Checklist

Use this checklist before adding a prompt to the library. A prompt should answer "yes" to all items.

- [ ] **Specific to Purview**: Does it reference Purview data sources, alert types, or policies by name? (Not generic)
- [ ] **Audience-aware**: Is the intended audience clear? (Analyst, manager, CISO, compliance officer, user)
- [ ] **Format-explicit**: Does it request a specific output format? (Timeline, table, bullet points, narrative)
- [ ] **Time-bounded**: Does it include a relevant time range? (Last N days, incident-centric, relative)
- [ ] **Grounded in identifiers**: Does it use specific policy names, user email addresses, or alert types? (Not placeholder text)
- [ ] **Confidence requested**: For investigation/hunting, does it ask for confidence levels and caveats?
- [ ] **Source-traced**: Does it request source attribution so findings are auditable?
- [ ] **Chainable**: Could another prompt take this prompt's output as input? (Structured output, clear data)
- [ ] **Tested**: Has it been run against sample Purview data? Does it produce useful output?
- [ ] **Documented**: Does it include a description, example, and tips for use?
- [ ] **Non-redundant**: Does the library already contain a similar prompt? If yes, merge or differentiate.
- [ ] **Concise**: Is the prompt readable and not overly complicated? (Can be understood in 30 seconds)

---

## 5. Prompt Versioning Approach

This library evolves as new Security Copilot capabilities and Purview features are integrated. Version prompts to track improvements.

### Versioning Scheme

Each prompt file includes a version block:

```yaml
---
title: "Investigate DLP Policy Violations"
category: Investigation
version: 1.2
updated: 2026-04-06
author: CSA / Security Copilot Champion
dependencies:
  - Purview DLP connector
  - Mail flow audit logs
---
```

**Version Format**: `MAJOR.MINOR`
- **MAJOR**: Significant change in prompt structure or approach (e.g., adding new required data source)
- **MINOR**: Refinement to wording or example (e.g., clarifying placeholder, adding tip)

### Changelog

Include a changelog at the end of each prompt file:

```
## Changelog

### v1.2 (2026-04-06)
- Added confidence level guidance
- Refined criteria for severity assessment
- Updated Purview policy names to current terminology

### v1.1 (2026-03-15)
- Expanded examples for information barrier violations
- Improved output formatting guidance

### v1.0 (2026-02-01)
- Initial release
```

### Contributing Improvements

When you develop a better version of a prompt:

1. Create a new file with the same name and bump the version number
2. Document what changed in the changelog
3. Include the original author as a reference
4. Test the new version against sample data
5. Submit for review before replacing the old version

---

## 6. Role-Based Prompt Mapping

Different personas use different prompt categories. Use this table to help team members find relevant prompts.

| Role | Primary Categories | Secondary Categories | Typical Workflow |
|------|-------------------|----------------------|------------------|
| **Incident Responder** | Triage, Investigation, Reporting | Enrichment, Summarization | Triage → Investigate → Enrich → Report |
| **Security Analyst** | Investigation, Hunting, Enrichment | Triage, Summarization, Advisory | Hunt → Investigate → Enrich → Advise |
| **CISO / Executive** | Summarization, Reporting | Hunting, Advisory | Receive summary → Review report → Review advisory |
| **Policy Administrator** | Operational, Advisory, Reporting | Triage, Hunting | Audit config → Hunt for gaps → Advise on tuning |
| **Compliance Officer** | Reporting, Operational | Enrichment, Summarization | Verify controls → Generate audit reports → Summarize for auditors |
| **Customer / Demo Audience** | Workshop/Demo, Triage, Investigation | Summarization, Advisory | Attend demo → Ask questions → See Copilot in action |
| **New Analyst** | Workshop/Demo, Triage, Summarization | Investigation, Enrichment | Learn capabilities → Practice triage → Graduate to investigation |

### Persona Details

**Incident Responder**:
- Owns alert triage and initial investigation
- Needs rapid severity assessment (Triage)
- Requires investigation prompts to build evidence chains
- Produces incident reports for archive and escalation
- Typical time per incident: 15-60 minutes

**Security Analyst**:
- Conducts deeper investigations and threat hunts
- Uses all categories except pure reporting
- Focuses on correlation and pattern detection
- Provides findings to IR team and management
- Typical time per task: 30 minutes to hours

**CISO / Executive**:
- Reviews summaries and reports, not raw data
- Cares about business impact, compliance, trends
- Informed by analyst and IR team output
- Makes strategic decisions about policy and resourcing
- Typical engagement: review prepared summaries and reports

**Policy Administrator**:
- Owns DLP, insider risk, and barrier configurations
- Uses operational prompts to audit and health-check
- Uses advisory prompts to prioritize improvements
- Generates reports for compliance and review
- Typical time: weekly/monthly reviews plus incident-driven tuning

**Compliance Officer**:
- Owns audit and regulatory reporting
- Needs auditable, sourced investigation findings
- Uses operational prompts to verify control deployment
- Generates formal reports for auditors and regulators
- Typical engagement: scheduled audits, incident-driven reporting

**Customer / Partner / New Analyst**:
- Learning Security Copilot capabilities
- Observes or participates in realistic scenarios
- Sees hands-on workflow from triage through reporting
- Workshop prompts provide structure and talking points
- Typical engagement: 30-60 minute demo or training session

---

## Design Philosophy

This taxonomy is grounded in the incident lifecycle and investigative methodology:

1. **Triage** gates investigation (fast, high false-positive tolerance)
2. **Investigation** builds evidence (thorough, high confidence requirement)
3. **Enrichment** adds context (interdisciplinary, connects security to business)
4. **Hunting** proactively searches (broad, ranked results)
5. **Advisory** recommends improvements (grounded in observed incidents)
6. **Reporting** documents findings (formal, auditable, compliance-ready)
7. **Summarization** translates for audiences (accessible, business-focused)
8. **Operational** verifies control health (technical, configuration-focused)
9. **Workshop/Demo** educates and demonstrates (narrative, hands-on)

Each category has a specific job. Combining them in workflows creates powerful investigative and operational capabilities.

---

## Pattern 9: The Operation Anchor

**Template**:
```
Analyze [Purview data source] for [entity] looking specifically for
these audit log operations: [EXACT_OPERATION_NAMES].
For each occurrence, provide [output format].
```

**When to Use**:
- SC is returning incomplete or incorrect results for activity-based queries
- You need to ensure SC queries the correct Unified Audit Log operation types
- Investigation requires precision on specific activity types (e.g., USB exfiltration, email forwarding)
- You want to avoid SC interpreting vague descriptions ("downloads") incorrectly

**Example with Purview**:
```
Analyze audit logs for user john@contoso.com in the past 30 days.
Look specifically for these operations: FileDownloaded,
FileSyncDownloadedFull, FileCreatedOnRemovableMedia,
FileCopiedToRemovableMedia, FileUploadedToCloud,
SharingSet, AnonymousLinkCreated, Send.

For each occurrence provide: timestamp, file name, destination,
and whether a DLP policy matched (DlpRuleMatch).
```

**Why This Pattern Exists**: Security Copilot sometimes fails to map natural language descriptions to the correct underlying Unified Audit Log operation names. When you say "downloads to USB," SC may not translate that to `FileCreatedOnRemovableMedia` or `FileCopiedToRemovableMedia`. By providing the exact operation names, you force SC to query the correct event types.

**Tips**:
- Reference the [Audit Log Operations Reference](../docs/reference/audit-log-operations.md) for exact names
- Combine operation names that serve the same investigation goal (e.g., all USB operations together)
- Include both the natural language description AND the operation name for clarity
- If SC returns no results, verify the operation name is correct for your workload

---

### Pattern 10: The SIT Anchor

**Template**:
```
Show [alert type] where the sensitive information type
"[EXACT_SIT_NAME]" was detected with [confidence level]
in [time range].
```

**When to Use**:
- You need alerts for a specific data type and SC is returning mixed results
- Policy tuning requires understanding which SIT triggered the match
- Compliance investigations require precise data type identification

**Example with Purview**:
```
Show DLP alerts where the sensitive information type
"Credit Card Number" or "U.S. Social Security Number (SSN)"
was detected with high confidence (>85%) in the past 7 days.
For each alert, show the match count, policy rule, and user.
```

**Why This Pattern Exists**: Saying "credit card data" may cause SC to match broader patterns. Using the exact SIT name `Credit Card Number` ensures SC queries the specific detection engine.

**Tips**:
- Reference the [Sensitive Information Types Reference](../docs/reference/sensitive-information-types.md) for exact names
- Include confidence thresholds to filter out low-quality matches
- For custom SITs, use the exact display name from your Purview compliance portal

---

## Error Recovery and Troubleshooting

When SC returns unexpected, incomplete, or incorrect results, use these recovery strategies before abandoning a prompt.

### Common SC Failures and Recovery Actions

| SC Behavior | Likely Cause | Recovery Action |
|------------|-------------|----------------|
| "I don't have enough information to answer" | Missing plugin, insufficient context, or alert not found | Verify Purview plugin is enabled. Check alert ID format. Try: `Summarize the DLP alert with ID <exactAlertId>` |
| Returns generic advice instead of specific data | Prompt too vague; SC defaulted to knowledge-based response | Add specific identifiers: user UPN, alert ID, exact policy name, time range |
| Returns data for wrong user or alert | Ambiguous prompt or context drift in long session | Start a new SC session. Use full UPN (user@domain.com) and exact alert ID |
| "I cannot access that data" | User lacks RBAC permissions, or plugin not enabled | Check user's Purview RBAC roles. Verify Purview plugin toggle in SC admin |
| Returns partial timeline (missing recent events) | Audit log lag (24-48 hours for UAL) | Note that recent events may not appear. Supplement with near-real-time DLP/IRM alerts |
| Misidentifies operation type | SC mapped natural language to wrong audit operation | Use the Operation Anchor pattern: include exact operation names in the prompt |
| Returns "no alerts found" when alerts exist | Alert ID format mismatch, or alert older than 30 days | Verify alert ID format. For older alerts, try broader queries with user UPN and date range |
| Inconsistent results across sessions | SC responses vary by session context and model state | Version all prompts. Include identical context setup at session start. Accept variability |
| Omits key fields from expected output | SC truncated response, or data not available | Break prompt into smaller pieces. Ask for one field at a time. Request specific fields explicitly |
| Conflates two users or incidents | Long session with multiple entities discussed | Start new session for each distinct investigation. Reference entities by full identifiers |

### Recovery Prompt Templates

**When SC cannot find an alert:**
```
I'm looking for a DLP alert. The alert ID is <alertId>.
If you cannot find this specific alert, show me all DLP alerts
for user <user> between <startDate> and <endDate> and I will
identify the correct one.
```

**When SC returns generic advice instead of data:**
```
I need specific data from our Purview environment, not general guidance.
Using the Purview plugin, show me the actual DLP alerts for
user <user>@contoso.com in the past 7 days.
List each alert with: alert ID, timestamp, policy name, and severity.
```

**When SC misidentifies activity types:**
```
The previous response may have included incorrect activity types.
Please re-analyze user <user>'s activities using specifically these
Unified Audit Log operations: [list exact operation names].
Do not interpret or expand the operation list.
```

**When SC output seems incomplete:**
```
Your previous response may be missing data. Please provide:
1. [Specific field 1]
2. [Specific field 2]
3. [Specific field 3]
If any of these are unavailable, state which ones and why.
```

### When to Start a New Session

Start a fresh SC session when:
- Investigation has been in the same session for more than 10 prompts
- SC responses are becoming less relevant or repeating itself
- You are switching from one user/incident to a different one
- SC is referencing context from a previous investigation in the same session
- You need to verify a finding independently (avoid confirmation bias)

---

## Future Enhancements

This taxonomy will evolve as:

- New Purview features are released (e.g., data sharing governance, privacy risk assessment)
- Security Copilot capabilities expand (e.g., threat hunting with external threat intelligence, automated response)
- Customer needs emerge (e.g., new compliance requirements, operational challenges)
- New operation types or SITs are added to Purview (update reference documents accordingly)

Watch this file for major updates. Subscribe to prompt library updates to stay current with new prompts and category additions.

---

**Last Updated**: April 2026
**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development
