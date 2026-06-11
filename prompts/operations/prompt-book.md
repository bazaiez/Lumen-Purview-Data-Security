# Operations and Configuration Prompt Book

**Validation Status:** All [VALIDATED] — Configuration guidance is a reliable SC capability
**Required Plugin:** Microsoft Purview
**Last Validated:** April 2026

Lumen — Purview Data Security: Operational tasks, policy setup guidance, configuration review, and deployment readiness.

**Note:** These prompts answer "how do I..." questions and guide operational decisions. SC leverages Purview documentation and best practices to support implementation work. Not all configurations are performed by SC (that's in the portal), but SC can guide, validate, and recommend.

**Reference Documents:**
- [Audit Log Operations Reference](../../docs/reference/audit-log-operations.md) — Exact operation names for monitoring prompts
- [Sensitive Information Types Reference](../../docs/reference/sensitive-information-types.md) — Exact SIT names for policy configuration
- [Plugin Dependency Map](../../docs/plugin-dependency-map.md) — Required plugins
- [Validation Matrix](../validation-matrix.md) — Testing status for each prompt

---

## 1. Sensitivity Label Configuration Guidance

**Title:** Label Configuration Planning

**Purpose:** Guide configuration of sensitivity labels aligned to your data classification strategy.

**When to Use:**
- Initial label setup
- Label configuration review
- Before deploying new label to organization
- Regulatory requirement for specific label

**Exact Prompt:**
```
Help me configure a new sensitivity label: [LABEL NAME: e.g., "Confidential"]

Required information:

1. Label purpose and use cases:
   - What data should get this label? (PII, financial, trade secrets, etc.)
   - Who typically applies this label (users, auto-labeling, both)?
   - Example documents that should have this label

2. Protection requirements:
   - Who should be able to view this data? (internal employees, specific departments, specific roles?)
   - Should this label restrict editing, sharing, copying, or downloading?
   - Should time-limited access be supported?

3. Regulatory/compliance requirements:
   - Any regulations require specific protections? (GDPR, HIPAA, PCI, etc.)
   - Minimum encryption level?
   - Required access logging?

4. Business requirements:
   - Can users share this externally (with customers, partners)?
   - Can admins/IT access this data for support?
   - Retention period for data with this label?

Based on this, recommend:
- Label encryption settings (AES-128 vs. AES-256)
- Access permissions and restrictions
- Rights management settings (IRM)
- Watermarking or content marking
- Sharing restrictions (internal only, whitelist, etc.)
- Retention/deletion policy

Also provide:
- Step-by-step configuration in Purview
- Testing checklist before deployment
- User communication talking points
```

**Expected Output:**
- Label configuration specification
- Encryption and protection recommendations
- Rights management settings
- Sharing and external access policy
- Retention/deletion settings
- Step-by-step portal configuration instructions
- Testing plan (how to validate label works)
- Rollout communication plan
- User training content (how to apply this label)

**Limitations:**
- SC can guide configuration; actual portal setup is admin task
- Some protections may not work in all applications
- User feedback may require configuration adjustments post-launch

**Tips:**
- Start with simple labels (add complexity iteratively)
- Test with small user group before org-wide rollout
- Document rationale for each protection setting (helps when justifying to users)

---

## 2. DLP Policy Configuration and Testing

**Title:** Data Loss Prevention Policy Setup

**Purpose:** Guide setup and testing of DLP policies to prevent accidental sensitive data sharing.

**When to Use:**
- Initial DLP policy deployment
- New data type protection requirement
- Policy review or adjustment
- Incident response (hardening policies)

**Exact Prompt:**
```
Help me configure a DLP policy for: [DATA TYPE: e.g., "Customer Credit Card Numbers"]

Policy context:

1. Data to protect:
   - What specifically are we protecting? (exact data type, format, examples)
   - Where is this data stored? (SharePoint, Teams, Exchange, OneDrive, etc.)
   - Scope: which sites/mailboxes/users does policy apply to?

2. Actions to prevent:
   - What should we block? (email external, cloud upload, print, copy to USB, etc.)
   - What should we log but allow (monitor without blocking)?
   - What user actions should trigger notification?

3. Exceptions and business needs:
   - Legitimate use cases (e.g., "Finance emails PO with card numbers to vendor")
   - Users/roles that need exceptions?
   - How to handle legitimate needs (approved override process?)

4. Regulatory/compliance requirements:
   - Are specific DLP controls required? (PCI-DSS, GDPR, HIPAA, etc.)
   - Required logging for audit?

Based on this, recommend:

- Rule conditions: what patterns/keywords/data types to detect?
- Sensitivity level (high confidence vs. low confidence rules)
- Actions: block, log, notify, or prompt user?
- Exceptions: approved groups, file types, locations
- User notifications: what message should users see?
- Incident reports: what gets logged and retained?

Also provide:
- Testing methodology (how to validate rule works before deployment)
- Tuning recommendations (balance protection vs. false positives)
- Monitoring and alert configuration
- Policy review cadence
```

**Expected Output:**
- DLP rule specifications
- Condition patterns (regex, keywords, sensitive data types)
- Action recommendations (block vs. notify vs. log)
- Exception handling
- User notification messaging
- Incident logging and alert configuration
- Step-by-step policy creation instructions
- Testing plan (how to verify rules detect intended data)
- Tuning guidance (expected false positive rate, adjustments)
- Monitoring/alert setup
- Review and maintenance schedule

**Limitations:**
- Some data patterns are hard to detect (context-dependent)
- Over-broad rules create false positives (user frustration)
- Policy effectiveness depends on user behavior (some will work around rules)

**Tips:**
- Start with high-confidence rules (well-defined data formats like credit cards)
- Monitor false positives closely first 30 days (adjust as needed)
- Provide approved channels for legitimate data sharing (don't just block)
- Tune regularly based on incident data and user feedback

---

## 3. Auto-Labeling Rule Configuration and Tuning

**Title:** Auto-Labeling Implementation

**Purpose:** Set up and optimize auto-labeling rules to improve label coverage without manual burden.

**When to Use:**
- Initial auto-labeling setup
- Expanding auto-labeling to new content types
- Performance review and tuning
- Addressing mislabeling issues

**Exact Prompt:**
```
Help me set up auto-labeling for: [DATA TYPE: e.g., "Customer Personal Data (PII)"]

Auto-labeling strategy:

1. Detection approach:
   - How should we identify this data type? (keywords, patterns, regex, sensitive data types)
   - Examples of content that should be auto-labeled
   - False positive scenarios (what might we mislabel?)

2. Confidence levels:
   - Should we use exact match (high confidence) or keyword matching (broader)?
   - What confidence threshold for applying label automatically?
   - What happens if confidence is below threshold (notify user, ignore, etc.)?

3. Scope:
   - Which workloads: SharePoint, Teams, Exchange, OneDrive, other?
   - Which sites/users does this apply to?
   - Any exclusions (certain file types, locations)?

4. Label to apply:
   - Which label should auto-labeling use?
   - Can users override the auto-label?
   - What notification should users see?

5. Tuning and monitoring:
   - How will we measure accuracy? (sample manual review)
   - What's acceptable false positive rate?
   - How often to review and adjust?

Based on this, recommend:

- Rule logic: exact patterns or keywords to match
- Confidence thresholds and scoring
- Content types to include/exclude
- Workload configuration
- User notifications
- Performance monitoring
- Tuning process (how to improve accuracy over time)

Also provide:
- Step-by-step portal configuration
- Testing methodology (validate rule accuracy before production)
- Initial accuracy baseline (sample and verify)
- Monitoring dashboard setup
- Adjustment process (how/when to refine rules)
```

**Expected Output:**
- Auto-label rule specifications
- Detection patterns and logic
- Confidence thresholds
- Scope (workloads, sites, users)
- Label assignments
- User notification messaging
- Baseline accuracy testing plan
- Monitoring and dashboard setup
- Monthly tuning process
- Expected coverage vs. accuracy (trade-off analysis)

**Limitations:**
- Context-based classification is hard (may require manual review)
- Some data types are ambiguous (legitimate vs. sensitive)
- Rules may need adjustment as content patterns evolve

**Tips:**
- Start with easy data types (credit cards, SSNs have obvious patterns)
- Test with pilot group before org-wide rollout
- Sample check accuracy monthly (don't assume it's perfect)
- Provide easy override for users (don't trap them)

---

## 4. Policy Audit and Configuration Review

**Title:** Policy Health Check

**Purpose:** Audit current DLP and label policies for completeness, correctness, and alignment with data protection goals.

**When to Use:**
- Quarterly governance review
- Post-incident hardening
- Policy consolidation (too many overlapping policies)
- Compliance audit preparation
- New regulatory requirement

**Exact Prompt:**
```
Audit our DLP and sensitivity label policies for completeness:

Current policy inventory: [LIST: e.g., "DLP-CreditCards, DLP-HealthData, Label-Confidential"]

For each policy, assess:

1. Correctness:
   - Is the rule detecting the intended data accurately?
   - Any false positives or false negatives?
   - Should scope be narrower or broader?

2. Completeness:
   - Are we protecting all variants of this data? (SSN: 123-45-6789 and 123456789?)
   - All relevant workloads covered? (SharePoint, Teams, Exchange, etc.)
   - All applicable users/departments included?

3. Alignment:
   - Does this policy align with our data protection strategy?
   - Any policies that conflict or overlap?
   - Are there protection gaps (data types with no policy)?

4. Effectiveness:
   - How many violations has this policy detected in past 90 days?
   - Trend: increasing, decreasing, stable?
   - User feedback: too strict, not helpful, or good?

5. Maintenance:
   - When was this policy last reviewed?
   - Any changes needed due to business/regulatory changes?
   - Is this policy still needed or can it be retired?

Based on audit, recommend:

- Policies to strengthen (tighten scope, increase detection)
- Policies to simplify or consolidate
- Policies with detection issues (tune patterns/thresholds)
- Policies to retire (no longer needed)
- Coverage gaps (new policies needed)
- Prioritization for changes (by impact and effort)

Also provide:
- Gap analysis (what data is unprotected)
- Consolidation opportunities (reduce policy count)
- Timeline for improvements
```

**Expected Output:**
- Policy audit scorecard (per policy: completeness, correctness, effectiveness)
- Gap analysis (data types without protection)
- Consolidation recommendations (policies to merge)
- Tuning recommendations (patterns, thresholds, scope)
- Retirement candidates (policies no longer needed)
- New policies needed (coverage gaps)
- Prioritized improvement roadmap
- Implementation timeline and effort

**Limitations:**
- Audit depends on accurate policy documentation
- Some policies may have legitimate reasons for current config
- Tuning requires testing before deployment

**Tips:**
- Run this quarterly (keep policies fresh)
- Get stakeholder input before major changes
- Document rationale for each policy (helps during review)
- Archive rather than delete policies (may need to restore)

---

## 5. Access Control and Permissions Validation

**Title:** Access Control Configuration Review

**Purpose:** Validate that Purview settings and label protections correctly enforce access controls.

**When to Use:**
- Compliance audit preparation
- Post-implementation validation
- Access control troubleshooting
- Regulatory requirement validation

**Exact Prompt:**
```
Validate our access control configuration for sensitive data:

Current setup:
- Data repositories: [SharePoint sites, Teams, Exchange mailboxes]
- Access control mechanism: [Azure AD groups, RBAC, custom roles, etc.]
- Label protection: [encryption, IRM, sharing restrictions]

Test and validate:

1. Identity and authentication:
   - How are users authenticated? (MFA enabled?)
   - Are service accounts isolated/restricted?
   - Are guest accounts properly scoped?

2. Role-based access control (RBAC):
   - Are roles correctly defined? (owner, editor, viewer, etc.)
   - Are users assigned appropriate roles per their job function?
   - Any over-privileged users or service accounts?

3. Label-based protections:
   - Are label encryption protections working?
   - Can users view sensitive labeled files if they don't have permission?
   - Are external sharing restrictions enforced?
   - Are time-limited permissions working (if configured)?

4. Audit logging:
   - Are access events logged?
   - Are label access/changes logged?
   - Is logging sufficient for compliance audit?

5. Compliance with standards:
   - Do we meet regulatory requirements? (GDPR, HIPAA, PCI, etc.)
   - Any required access controls missing?

Test findings:

- For each control, rate: Working / Partially Working / Not Working
- For issues found, recommend: Configuration change / Additional training / Policy update / Technical investigation

Provide:
- Control validation checklist
- Test results (per control)
- Issues and remediation recommendations
- Urgency/timeline for fixes
- Test evidence for compliance auditors
```

**Expected Output:**
- Access control validation checklist
- Test results per control (working/not working)
- Issues identified with severity
- Remediation recommendations
- Effort/timeline for fixes
- Evidence documentation (for auditor)
- Post-remediation validation plan
- Continuous monitoring process

**Limitations:**
- Some controls require technical testing (SC can guide, not execute)
- User behavior may bypass controls (not purely technical)
- Regulations may require manual controls outside of Purview

**Tips:**
- Do this annually (at minimum)
- Document all tests and results (for audit evidence)
- Remediate high-severity issues quickly
- Test changes before deploying to production

---

## 6. Deployment Readiness Assessment

**Title:** Pre-Launch Validation Checklist

**Purpose:** Validate organization readiness before deploying Purview security features to users.

**When to Use:**
- Before label taxonomy rollout
- Before DLP policy enforcement
- Before feature/capability enablement
- Quarterly go-live readiness check

**Exact Prompt:**
```
Assess deployment readiness for: [FEATURE: e.g., "Mandatory email labeling"]

Deployment plan:
- Target users: [number and roles]
- Rollout timeline: [phased or big bang?]
- Success criteria: [adoption rate, compliance %, incidents avoided, etc.]

Validate readiness:

1. Technical readiness:
   - Are systems configured correctly?
   - Have we tested with representative user group?
   - Are there known compatibility issues with user tools/apps?
   - Is IT support trained and ready?

2. Process readiness:
   - Do we have documented procedures for users?
   - Is IT helpdesk trained to support users?
   - Do we have escalation procedures if issues arise?
   - Is governance in place (who approves exceptions, etc.)?

3. User readiness:
   - Has user communication been sent?
   - Have users received training?
   - Do users understand why this is important?
   - Are there resource/tools available for help?

4. Monitoring and support readiness:
   - Do we have monitoring/alerts set up?
   - Is there a way to detect widespread issues?
   - Do we have rollback procedure if needed?
   - Is on-call support scheduled?

5. Success metrics and acceptance criteria:
   - How will we measure success?
   - What issues would cause rollback?
   - How long is "safe mode" (can we adjust if needed)?

Readiness assessment:

For each area: Green (ready) / Yellow (risky) / Red (not ready)

If any Red items, recommend:
- What must be done before launch
- What is acceptable risk to proceed
- Contingency plan if issues arise

Provide:
- Readiness scorecard
- Go/no-go recommendation
- Critical path items (must fix before launch)
- Launch day checklist
- Contingency/rollback procedures
```

**Expected Output:**
- Readiness scorecard (by category: Green/Yellow/Red)
- Go/no-go recommendation
- Critical issues and must-fix items
- Launch day checklist
- Rollback procedures
- Support team readiness checklist
- Communication plan
- Monitoring and alert configuration
- Success metrics and acceptance criteria
- Post-launch support plan (24h, 7d, 30d check-ins)

**Limitations:**
- Some readiness factors are organizational (not pure tech)
- Perfect readiness is rare; acceptable risk varies per org
- Post-launch issues may surface despite preparation

**Tips:**
- Do readiness assessment 2 weeks before launch
- Address Red items before launch
- Have rollback procedure documented (even if not used)
- Plan for hiccups (support team should expect calls)

---

## 7. User Training and Adoption Program Design

**Title:** Security Awareness and User Training

**Purpose:** Design and execute user training to drive label adoption and policy compliance.

**When to Use:**
- Before label or policy rollout
- Response to high violation rates
- Quarterly training refresher
- New employee onboarding

**Exact Prompt:**
```
Design a user training program for: [FEATURE: e.g., "Sensitivity Labels"]

Training context:

1. Target audience:
   - Who needs training? (all users, specific departments, roles)
   - User tech-savviness (IT staff vs. general staff)
   - Department-specific needs (e.g., Finance handles sensitive data differently)

2. Current adoption/compliance:
   - What % of users apply labels? (if applicable)
   - What % comply with DLP policies?
   - What are common mistakes or questions?

3. Training approach:
   - Format: videos, live training, self-paced, documentation?
   - Length: 5 min micro-training or 30 min comprehensive?
   - Frequency: one-time or recurring?

4. Business outcome:
   - What should users be able to do after training?
   - How will we measure success? (adoption rate, compliance %, user satisfaction)

Design training program including:

1. Training content:
   - Key messages (why this matters, business impact)
   - Step-by-step how-to (label application, policy compliance, exceptions)
   - Scenarios and examples (common situations in their role)
   - Q&A and troubleshooting

2. Training delivery:
   - Format and channels (video, email, meeting, wiki, etc.)
   - Rollout schedule (when to train)
   - Who delivers training (IT, managers, champions, etc.)

3. Support materials:
   - Quick reference guides (email, poster, desktop card)
   - FAQ addressing common questions
   - Support contacts and escalation

4. Measurement and reinforcement:
   - Training completion tracking
   - Knowledge checks (quiz or self-assessment)
   - Monitoring adoption post-training
   - Reinforcement messages (if adoption is low)

5. Role-specific training:
   - IT/security staff: additional technical context
   - Data stewards: governance and exception handling
   - Managers: accountability and team expectations
   - General staff: basic how-to and why it matters

Provide:
- Training curriculum with learning objectives
- Sample training materials (scripts, slides, videos)
- Delivery plan with timeline
- Measurement approach
- FAQ and support procedures
```

**Expected Output:**
- Training curriculum with learning objectives
- Training content (scripts, slides, videos, documentation)
- Role-specific training modules
- Delivery schedule and ownership
- Support materials (quick guides, FAQs, posters)
- Completion tracking mechanism
- Knowledge assessment (quiz or self-check)
- Post-training monitoring plan (adoption, compliance)
- Reinforcement campaign (if adoption is low)
- Training effectiveness measurement

**Limitations:**
- Training alone doesn't guarantee adoption (process/tools matter too)
- Some users learn better via different formats (need multiple options)
- Recurring training needed (people forget)

**Tips:**
- Include user success stories (relatable examples)
- Make training mandatory initially (shows importance)
- Provide support during and after training
- Use positive reinforcement (celebrate early adopters)

---

## 8. Incident Response Procedure Documentation

**Title:** Security Incident Response Playbook

**Purpose:** Document procedures for responding to data security incidents (breach, policy violation, suspicious activity).

**When to Use:**
- During incident response (guide actions)
- Post-incident (formalize lessons learned)
- Compliance requirement (documented procedures)
- New security team member onboarding

**Exact Prompt:**
```
Document incident response procedures for: [INCIDENT TYPE: e.g., "Suspected Data Breach"]

Response context:

1. Incident type definition:
   - What constitutes this incident? (criteria for escalation)
   - Who should detect it? (automated alert, user report, security team)
   - Severity levels (Critical, High, Medium, Low)

2. Key roles and responsibilities:
   - Who is incident commander?
   - Who investigates (security team, forensics)?
   - Who communicates to leadership/customers?
   - Who makes decisions (contain, escalate, notify)?

3. Procedural steps:

   - DETECT & ALERT:
     * How is incident reported/detected?
     * Who notifies the incident response team?
     * Immediate actions (first 30 minutes)?

   - ASSESS & CONTAIN:
     * How to determine scope (what data, how many users)?
     * Containment actions (block accounts, reset passwords, etc.)?
     * Preserve evidence (don't delete logs/data)?

   - INVESTIGATE:
     * Timeline of events (when did it start, when detected)?
     * Root cause analysis (what allowed this)?
     * Impact assessment (data at risk, business impact)?

   - COMMUNICATE:
     * Internal: who needs to be notified and when?
     * External: customer/regulatory notification if required?
     * Media: is external communication needed?

   - REMEDIATE:
     * What changes prevent recurrence?
     * Timeline for fixes
     * Who owns remediation

   - CLOSE & REVIEW:
     * Post-incident review (what did we learn?)
     * Lessons learned and process improvements
     * Stakeholder signoff and archive

4. Escalation and decision-making:
   - When to escalate to CISO/CEO?
   - When to involve legal counsel?
   - When to notify regulators?
   - Decision criteria for customer notification

5. Tools and resources:
   - What tools support investigation? (SC, Purview, forensics tools, etc.)
   - Contact list (security team, legal, PR, customers)
   - Evidence preservation and chain of custody procedures

Provide:
- Documented incident response procedures
- Decision tree (assess -> contain -> communicate -> remediate)
- Role assignments and escalation
- Timeline expectations (MTTR targets)
- Communication templates (internal and external)
- Regulatory notification checklist
- Post-incident review template
- Training for response team
```

**Expected Output:**
- Incident response playbook (step-by-step procedures)
- Decision tree for severity/escalation
- Role assignments and escalation chain
- Contact list and notification procedures
- Communication templates (internal and external)
- Regulatory notification checklist and deadlines
- Evidence preservation procedures
- Tools and systems to use
- Timeline expectations (detect to contain to notify to resolve)
- Post-incident review template
- Training agenda for response team

**Limitations:**
- Procedures must be customized per organization
- Regulatory requirements vary by jurisdiction
- Procedures may need adjustment during incident

**Tips:**
- Document multiple incident types (not just breaches)
- Test procedures regularly (tabletop exercises)
- Keep contact list updated
- Involve legal and PR in planning

---

## 9. Continuous Improvement and Governance Process

**Title:** Security Policy and Control Review Cycle

**Purpose:** Establish ongoing review, monitoring, and improvement of security policies and controls.

**When to Use:**
- Security program establishment
- Annual governance planning
- Policy owner assignment
- Compliance framework setup

**Exact Prompt:**
```
Design governance and continuous improvement process for: [AREA: e.g., "Data Protection Policies"]

Current state:
- Policies in place: [list]
- Policy owners: [assigned/unassigned]
- Review frequency: [never/annually/quarterly]
- Metrics/monitoring: [what's tracked]

Design ongoing governance:

1. Policy lifecycle:
   - How often are policies reviewed? (quarterly, annually?)
   - Who reviews (policy owner, leadership, full team)?
   - Process for policy change/approval
   - Communication of policy updates

2. Policy ownership:
   - Who owns each policy? (clear accountability)
   - Owner responsibilities (review, training, enforcement)
   - Owner escalation path (issue handling)

3. Monitoring and metrics:
   - What metrics indicate policy effectiveness?
   - How frequently monitored (daily, weekly, monthly)?
   - Who receives reports (security team, leadership)?
   - Alerting thresholds (when to escalate)

4. Compliance and testing:
   - How do we validate policies are working?
   - Audit approach (self-assessment, third-party, continuous)
   - Testing frequency and who executes
   - Evidence collection for compliance

5. Feedback and adjustment:
   - How do we collect feedback? (user surveys, incident data, audit findings)
   - Process for policy adjustment based on feedback
   - Approval workflow for changes
   - Communication of updates

6. Escalation and governance:
   - What issues require escalation to leadership?
   - Governance committee (who, when, how often)?
   - Risk register updates and reviews
   - Budget/resource planning

7. Documentation and training:
   - Policy documentation standards
   - Training for policy owners
   - Training for users (policy awareness)
   - Documentation management system

Provide:
- Policy lifecycle process and timeline
- Policy owner role description and checklist
- Monitoring dashboard and metrics definitions
- Audit and testing schedule
- Governance meeting cadence and agenda
- Escalation criteria and procedures
- Annual review planning template
- Continuous improvement backlog management
```

**Expected Output:**
- Policy governance framework
- Policy owner role and responsibilities
- Monitoring metrics and dashboard
- Audit and testing schedule (annual)
- Governance meeting cadence and agenda
- Risk register and escalation procedures
- Continuous improvement backlog
- Annual planning and budget cycle
- Policy documentation template
- Training plan for policy owners

**Limitations:**
- Governance requires consistent execution (easy to lapse)
- Some policies may have infrequent changes (review burden)
- Regulatory requirements may dictate review frequency

**Tips:**
- Assign clear policy owners (accountability)
- Automate monitoring where possible (less manual work)
- Review quarterly minimum (even if no changes)
- Connect to business objectives (why policies matter)

---

## 10. Security Tool Integration and Data Architecture

**Title:** Purview Data Sharing and Integration Planning

**Purpose:** Plan how Purview data flows to other security tools and systems (SIEM, SOC, dashboards, etc.).

**When to Use:**
- Tool consolidation or replacement
- SIEM/SOC integration planning
- Executive dashboard setup
- Data architecture review

**Exact Prompt:**
```
Plan Purview integration with existing security infrastructure:

Current tools and systems:
- SIEM: [e.g., Splunk, Sentinel]
- SOC/alerting: [tool/process]
- Executive dashboards: [Power BI, Tableau, other]
- Risk management: [system]
- Incident response: [ticketing system]

Integration requirements:

1. Data sharing:
   - What Purview data should flow to each system?
     (alerts, audit logs, risk scores, policy violations, etc.)
   - Frequency (real-time, daily, weekly)?
   - Format (API, webhook, file export, etc.)?

2. Alert and response:
   - What alerts from Purview should trigger SOC actions?
   - How should alerts escalate? (to whom, what system?)
   - Integration with incident ticketing system?

3. Executive reporting:
   - What KPIs from Purview for executive dashboard?
   - Frequency of updates (hourly, daily, weekly)?
   - Who has access to dashboard?

4. Context enrichment:
   - What external data should enrich Purview data?
     (threat intelligence, user directory, asset inventory?)
   - How to link events across tools?

5. Compliance and audit:
   - What data must be retained and how long?
   - Chain of custody for evidence data?
   - Access controls for sensitive investigation data?

6. Cost and performance:
   - API call volumes and costs?
   - Storage requirements for historical data?
   - Performance impact on source and destination systems?

Design integration:

- Data flow diagram (Purview -> other systems)
- API/integration method per connection
- Data schema and field mapping
- Alert routing and escalation logic
- Failure handling and retry logic
- Access controls and authentication
- Audit logging of data sharing
- Performance and cost estimates
- Implementation timeline

Provide:
- Integration architecture diagram
- Technical specifications per connection
- API/webhook documentation
- Data schema definitions
- Alert routing rules
- Access control policy
- Testing and validation plan
- Deployment checklist
```

**Expected Output:**
- Integration architecture diagram
- Data flow specifications (what, how often, format)
- API/connection specifications
- Data schema and field mapping
- Alert routing logic
- Access control and authentication
- Audit logging and compliance
- Performance and cost estimate
- Implementation plan with timeline
- Testing and validation checklist
- Runbook for operations team

**Limitations:**
- Integrations depend on API availability (not all tools supported)
- Data consistency across systems may be challenging
- Some real-time integrations have latency

**Tips:**
- Start with high-value integrations (SIEM alerts, executive dashboard)
- Plan for data enrichment (add context from other sources)
- Consider data retention and compliance needs
- Test integration thoroughly before production

---

## Tags and Patterns

- **[VALIDATED]** Prompts 1, 2, 3, 4, 7, 10: Core operational and configuration guidance. SC provides step-by-step guidance that operators implement in Purview portal.
- **[VALIDATED]** Prompts 5, 6, 8, 9: Validation, readiness, and process design. Real-world implementations use these patterns.
- **[ASPIRATIONAL]** Some cross-system integrations (Prompt 10) may require custom development or third-party connectors.

All prompts assume Purview is deployed and operational. Scope configuration steps to match your org's Purview SKU and licensing level.

