# Executive Reporting Prompt Book

**Validation Status:** 7 [VALIDATED] / 1 [ASPIRATIONAL]
**Required Plugin:** Microsoft Purview
**Last Validated:** April 2026

Lumen — Purview Data Security: Leadership summaries, board reports, and incident briefs.

**Note:** These prompts focus on SC's ability to synthesize and translate technical findings into executive language. SC consumes raw Purview data and outputs leadership-ready narratives, risk assessments, and recommendations.

**Reference Documents:**
- [Plugin Dependency Map](../../docs/plugin-dependency-map.md) — Required plugins
- [Validation Matrix](../validation-matrix.md) — Testing status for each prompt

---

## 1. Monthly Data Security Risk Summary for Leadership [VALIDATED]

**Title:** Executive Risk Dashboard Brief

**Purpose:** Provide monthly snapshot of organizational data security posture for C-level review and board reporting.

**When to Use:**
- Monthly security briefing to CISO or security committee
- Board reporting on data security metrics
- Executive dashboard input
- Quarterly shareholder/investor communications

**Exact Prompt:**
```
Prepare monthly data security executive summary for [MONTH]:

Format: 1-2 page executive brief suitable for CISO/Board review

Include:

1. Data risk trending:
   - Overall organizational data risk score (trending up/down/stable?)
   - Key risk drivers (what's pushing the score?)
   - Comparison to previous month

2. Top 5 risks requiring leadership attention:
   - Risk description in business terms (not technical jargon)
   - Severity and why it matters (financial, compliance, reputation impact)
   - Current remediation status
   - Timeline to resolution

3. Compliance and regulatory highlights:
   - Compliance status for key regulations (GDPR, CCPA, HIPAA, etc.)
   - Any new gaps or incidents
   - Audit readiness level

4. Data protection metrics:
   - Sensitivity label coverage % (trending)
   - Policy violations and trend (up/down)
   - User security incidents or anomalies

5. Incident summary:
   - Any new security incidents involving data?
   - Severity, impact, and remediation status
   - Lessons learned or policy changes needed

6. Key wins this month:
   - Improvements to data protection (new policies, improved coverage, etc.)
   - Successful audit results or compliance certifications
   - User security training completion rates

7. Resources requested:
   - Budget, headcount, or tool investments needed
   - Timeline and expected ROI

Include visual aids: trend chart, risk heat map, KPI scorecard.
```

**Expected Output:**
- 1-2 page executive brief with clear visual aids
- Business-language risk descriptions
- Trending analysis (month-over-month or quarter-over-quarter)
- Recommendations with resource requirements
- Compliance status summary
- Key metrics dashboard (adoption, violations, incidents)
- Forward look (what to watch next month)
- Ready for board presentation or shareholder communication

**Limitations:**
- Monthly summary is point-in-time; ongoing incidents may escalate post-report
- Executive brief can't include all details; recommend reference documents
- Some risks may lack clear financial impact quantification

**Tips:**
- Lead with trending and key wins (builds confidence)
- Use simple metaphors for non-technical audience ("data locked in the vault" vs. "AES-256 encryption")
- Include 1-2 customer-facing examples (shows real-world relevance)
- Prepare detailed reference documents for deep-dive questions

---

## 2. Incident Impact Brief for Leadership [VALIDATED]

**Title:** Breach or Incident Executive Summary

**Purpose:** Communicate incident scope, impact, and response status to leadership in real-time or post-incident.

**When to Use:**
- During active incident (keep leadership informed hourly/daily)
- Post-incident brief (72 hours after containment)
- Investor/customer notification preparation
- Board emergency session briefing

**Exact Prompt:**
```
Prepare incident executive brief for: [INCIDENT NAME/DATE]

This brief will be delivered to [CISO/CEO/Board/Customers].

Required sections:

1. Incident summary:
   - What happened (in 1-2 plain English sentences)?
   - When did it occur and when was it discovered?
   - Who discovered it and current response status?

2. Scope and impact:
   - How many customers/employees affected?
   - What types of data were exposed (PII, payment data, health data)?
   - Estimated volume of data at risk
   - Business impact (revenue at risk, operational disruption)?

3. Root cause (if known):
   - What enabled this incident? (user error, policy gap, technical vulnerability)
   - Was this preventable with existing controls?
   - Likelihood of recurrence

4. Response timeline:
   - Discovery -> reporting -> containment: how long each step?
   - Current status (contained, investigating, remediating)
   - Estimated resolution timeline

5. Actions taken:
   - What did we do to contain the incident?
   - Who was notified (internal teams, customers, regulators)?
   - Forensic investigation status

6. Customer/stakeholder impact:
   - Do customers need to take action (password reset, credit monitoring)?
   - External notification requirements (regulatory, contractual)?
   - Legal/compliance implications
   - Reputational or financial impact estimate

7. What we're doing to prevent recurrence:
   - Policy or process changes
   - Technology improvements needed
   - Timeline for prevention measures
   - Resource requirements

8. Questions/concerns to expect:
   - What will customers/regulators ask?
   - Prepared responses to hard questions
   - Who is point of contact for escalations?

Tone: Transparent, factual, action-oriented. Acknowledge severity while demonstrating control.
```

**Expected Output:**
- 2-3 page executive incident brief
- Timeline (discovery through response)
- Impact quantification (affected users, data types, business impact)
- Root cause summary
- Response actions (what we did right)
- Prevention plan with timeline and resources
- Customer/regulatory communication draft
- FAQ for leadership to anticipate questions
- Escalation plan (who approves customer/investor notification)

**Limitations:**
- Incident brief evolves as investigation progresses
- Root cause may be unclear until forensics complete
- Regulatory requirements may constrain what can be communicated publicly

**Tips:**
- Update brief daily during active incident
- Have legal review before external communication
- Lead with "we detected it and contained it quickly" (builds trust)
- Prepare detailed forensic report separately (reference in brief)

---

## 3. Data Compliance Audit Readiness Report [VALIDATED]

**Title:** Compliance Certification and Audit Preparation

**Purpose:** Demonstrate data security and compliance posture to regulators and auditors.

**When to Use:**
- Annual audit preparation (SOX, HIPAA, GDPR)
- Regulatory inquiry response
- Customer audit questionnaire completion
- Vendor/partner security assessment

**Exact Prompt:**
```
Prepare compliance audit readiness report for: [REGULATION: SOX/HIPAA/GDPR/CCPA]

Audience: Internal audit team, external auditors, regulatory bodies

Required deliverables:

1. Regulatory requirements summary:
   - Key requirements relevant to data security
   - Our organization's scope (which systems/data are in scope)

2. Control environment assessment:
   - Access controls: How do we ensure only authorized users access sensitive data?
   - Logging: Are all data access events logged and retained?
   - Encryption: What encryption standards do we use?
   - Data retention/deletion: How do we enforce retention policies?
   - Incident response: Do we have documented incident response procedures?

3. Compliance status by requirement:
   - Requirement: [specific control]
   - Status: Compliant / Non-Compliant / Partially Compliant / Compensating Control
   - Evidence: How can we prove compliance? (audit logs, policy documents, certifications)
   - Timeline if not yet compliant

4. Testing evidence:
   - Sample access logs showing proper auditing
   - Sample of labeled/classified data showing protection
   - Evidence of access reviews and user validation
   - Incident response documentation (if any incidents occurred)
   - Encryption validation (certificates, key management proof)

5. Gaps and remediation plan:
   - Any non-compliant controls
   - Root cause (policy gap, implementation gap, process gap)
   - Remediation steps and timeline
   - Resource requirements
   - Interim compensating controls (if remediation is in progress)

6. Third-party assurance:
   - Any external certifications or attestations? (ISO 27001, SOC 2, etc.)
   - Audit results from previous years

7. Continuous monitoring:
   - How do we continuously validate compliance? (quarterly reviews, automated testing)
   - Evidence of ongoing monitoring

Format ready for auditor review and regulatory submission.
```

**Expected Output:**
- Compliance framework mapping (regulation -> controls -> evidence)
- Status scorcard: % compliant, gaps, remediation timeline
- Control testing evidence (audit logs, screenshots, test results)
- Gap analysis with remediation plan
- Certifications and audit history
- Continuous monitoring process documentation
- Executive summary for regulatory submission
- Detailed workpapers for auditor review

**Limitations:**
- Audit readiness requires controls beyond SC's visibility (contracts, DPAs, etc.)
- Some evidence requires manual collection or system queries outside SC
- Auditors may request additional testing beyond SC's scope

**Tips:**
- Ask SC to highlight your strongest controls (build confidence)
- Request SC to flag remediation items with target dates (shows progress)
- Prepare auditor management letter responses if prior findings exist
- Include customer/partner attestations if available

---

## 4. Data Breach Notification and Investor/Customer Communication

**Title:** Breach Notification and Disclosure Preparation

**Purpose:** Prepare legally compliant and transparent breach notification for customers, investors, and regulators.

**When to Use:**
- Post-incident (after forensics and containment)
- Regulatory notification requirement (GDPR 72h, CCPA requirements, etc.)
- Investor/shareholder communication
- Media statement preparation
- Board notification

**Exact Prompt:**
```
Prepare breach notification communications:

Incident: [INCIDENT SUMMARY]
Affected parties: [Customers/Employees/Partners]
Regulatory deadlines: [GDPR 72-hour rule, state notification laws, etc.]

Deliver multiple communication versions:

1. Regulatory notification (for regulators):
   - Technical facts: what data, when, scope, root cause
   - Access: who had unauthorized access?
   - Remediation: what we did to contain and prevent recurrence
   - Impact: number of individuals, data categories
   - Legal/compliance references

2. Customer notification email:
   - What happened (plain English, non-technical)
   - What data was affected (specific to their data, not industry-wide guess)
   - What we're doing about it
   - What customers should do (credit monitoring, password reset, etc.)
   - Timeline of events (discovery to notification)
   - Contact information for questions
   - Apology and commitment to security

3. Investor/media statement:
   - Business impact summary (financial, operational)
   - Incident response effectiveness (detected quickly, contained promptly)
   - Long-term security measures and investment
   - Forward-looking statement about data protection

4. FAQ for support team:
   - Expected customer questions and prepared answers
   - How to handle angry/concerned customers
   - Escalation procedures

5. Legal/compliance checklist:
   - Required notifications (regulators, credit bureaus, customers)
   - Timelines per jurisdiction
   - Compliance requirements (disclosures, documentation, etc.)
   - Litigation readiness (attorney involvement)

Tone guidance:
- To regulators: Factual, thorough, compliant
- To customers: Empathetic, transparent, reassuring about next steps
- To investors: Professional, controlled, forward-focused

All communications must be reviewed by legal counsel before sending.
```

**Expected Output:**
- Multiple communication templates (regulator, customer, investor, media)
- Impact assessment and affected individual counts
- Timeline for notifications per jurisdiction
- Credit monitoring/identity protection offer if warranted
- FAQ with prepared responses
- Media statement options
- Legal compliance checklist
- Board briefing materials
- Document retention and chain of custody

**Limitations:**
- SC cannot provide legal advice; all communications must be attorney-reviewed
- Regulators may have specific requirements beyond SC's scope
- Tone and messaging reflect organization values (SC proposes, leadership decides)

**Tips:**
- Have legal counsel lead communication strategy
- Be transparent about what you know and don't know yet
- Offer meaningful remediation (credit monitoring, security improvements)
- Prepare multiple versions (customer communication may vary by jurisdiction)

---

## 5. Security Metrics and KPI Reporting for Board

**Title:** Quarterly Data Security Metrics Dashboard

**Purpose:** Report quantifiable security metrics and KPIs to board of directors or executive committee.

**When to Use:**
- Quarterly board or audit committee meeting
- Annual security program review
- Justification for security budget/investment
- Investor relations / shareholder communications

**Exact Prompt:**
```
Prepare quarterly data security metrics report for board:

Quarter: [Q#/Year]
Audience: Board of Directors / Audit Committee / Executive Sponsor

Metrics to include:

1. Data protection metrics:
   - % of sensitive data labeled with security classification
   - Trend: how is this changing quarter-over-quarter?
   - Target: what's our goal?
   - Risk: what's the impact of unlabeled data?

2. Access control metrics:
   - User access reviews completed: % on schedule
   - Orphaned accounts removed: #, days since termination
   - Privileged access governance: % of high-risk accounts reviewed
   - Approval workflow compliance: % of access granted with documented approval

3. Policy compliance metrics:
   - DLP policy violations: count, trend, root cause
   - Email external share violations: count, pattern analysis
   - Unclassified data handling: count, remediation status
   - Policy exception requests: count, approval rate

4. Security incident metrics:
   - Data-related incidents: count, severity, resolution time
   - Near-miss/DLP blocks: count, prevented incidents (ROI of controls)
   - User training completion: % of workforce trained on data security
   - Phishing click rate: % baseline (leading indicator of breach risk)

5. Compliance metrics:
   - Audit readiness: % of requirements validated
   - Regulatory findings: open items, remediation timeline
   - Third-party assessments: SOC 2, ISO 27001, etc.
   - Customer audit results: any findings?

6. Financial metrics:
   - Cost of security incidents: sum of remediation, notification, legal costs
   - Security budget vs. risk (are we spending enough?)
   - Cost avoidance: estimated incidents prevented by controls
   - ROI of new security tools/initiatives

7. Benchmarking:
   - How do our metrics compare to peer organizations?
   - Industry best practices
   - Are we ahead or behind peer average?

8. Trend analysis:
   - Which metrics are improving vs. declining?
   - Leading indicators of problems (e.g., training completion dropping)
   - Success stories and improvements to highlight

9. Risk assessment:
   - Overall data security risk: Low/Medium/High/Critical
   - Top 3 risks and remediation progress
   - Resource needs to reduce risk

10. Forward look:
   - Next quarter priorities
   - New capabilities or initiatives
   - Resource or budget requests

Provide visual presentation with trend charts, heat maps, and executive summary.
```

**Expected Output:**
- 5-10 page board-level presentation with visuals
- KPI dashboard with trends and benchmarks
- Business-language risk assessment
- Compliance status and audit readiness
- Incident summary and lessons learned
- Budget and resource justification
- Roadmap for next quarter
- Q&A preparation (anticipated board questions)

**Limitations:**
- Some metrics may require data from multiple systems
- Benchmarking data may not be available
- Financial impact quantification is often estimated, not exact

**Tips:**
- Lead with positive trends and wins
- Use visual presentation (more impactful than tables)
- Connect metrics to business risk (not just security metrics)
- Prepare detailed workpapers if board wants to drill down

---

## 6. CISO-to-CTO Technical Briefing on Data Risk Architecture

**Title:** Technical Deep-Dive Data Risk Report

**Purpose:** Provide CISO or CTO with technical details on data risk landscape, architecture risks, and remediation options.

**When to Use:**
- CISO/CTO strategy discussion
- Architecture review meeting
- Technology investment decision
- Security architecture documentation

**Exact Prompt:**
```
Prepare technical briefing for CISO/CTO on data risk architecture:

Audience: CISO, CTO, Enterprise Architect (technical but non-implementation)

Sections:

1. Data landscape overview:
   - Where is sensitive data stored? (cloud, on-premise, hybrid, SaaS)
   - Data volumes and growth trajectory
   - Data lifecycle (creation -> retention -> deletion)
   - Critical data repositories and risk ranking

2. Current protection architecture:
   - Access control mechanisms (identity, RBAC, ABAC)
   - Encryption standards and key management
   - Data classification/labeling approach
   - Monitoring and alerting capabilities

3. Risk analysis:
   - Architectural risks (single points of failure, unprotected data flows)
   - Operational risks (policy enforcement gaps, manual processes)
   - Compliance risks (regulatory requirements not met)
   - Threat modeling: who could compromise this data and how?

4. Gap analysis:
   - Current vs. desired state architecture
   - Specific gaps by risk category
   - Compliance gaps vs. industry standards
   - Technical debt or legacy system risks

5. Remediation options:
   For each major gap:
   - Option A: Build/configure internally
   - Option B: SaaS/managed service
   - Option C: Hybrid approach
   - Pros/cons, cost, timeline, skill requirements

6. Technology recommendations:
   - Purview Data Security capabilities to expand
   - Additional tools or services needed
   - Integration requirements
   - Staffing/training needed

7. Roadmap:
   - Phase 1 (Q1/Q2): Quick wins, high-impact, low-effort
   - Phase 2 (Q2/Q3): Medium complexity, high value
   - Phase 3 (Q3/Q4+): Strategic, transformational changes
   - Timeline and resource requirements per phase

8. Financial analysis:
   - Cost of current risk (incidents, compliance failures, business impact)
   - Cost of proposed remediation
   - ROI and payback period
   - Justification for investment

Include architecture diagrams, gap heat maps, and risk/reward analysis.
```

**Expected Output:**
- Technical architecture assessment
- Current state vs. desired state diagrams
- Risk analysis with threat modeling
- Gap analysis and remediation options
- Technology evaluation and recommendations
- Phased roadmap with timelines
- Cost-benefit analysis
- Resource requirements (people, tools, budget)
- Risk reduction trajectory

**Limitations:**
- SC provides assessment; implementation requires technical team
- Some risks require context from broader security program
- Technology choices involve business decisions beyond SC's scope

**Tips:**
- Include security architecture diagrams (easier to understand than text)
- Provide multiple remediation options (don't prescribe single solution)
- Quantify risk reduction (what % of data risk is eliminated per phase?)
- Pair with business case (ROI and payback) for approval

---

## 7. Customer Security Posture Report (for Vendor Security Assessment)

**Title:** Customer-Facing Security Assessment Report

**Purpose:** Demonstrate your organization's data security posture to customer security teams or auditors.

**When to Use:**
- Customer RFP/RFQ responses
- Vendor security assessment questionnaire
- Partner due diligence process
- Customer audit or assessment

**Exact Prompt:**
```
Prepare customer-facing security posture report:

Audience: Customer CISO/security team, auditors, procurement

Report should demonstrate our data security capabilities and maturity:

1. Executive summary:
   - Our commitment to data security
   - Key achievements and certifications
   - Investment in security and compliance

2. Data protection capabilities:
   - How we classify and protect sensitive data
   - Encryption standards and key management
   - Access controls and identity management
   - Monitoring and incident response

3. Compliance and certifications:
   - Relevant certifications (ISO 27001, SOC 2, etc.)
   - Regulatory compliance (GDPR, HIPAA, CCPA, etc.)
   - Industry-specific standards (PCI-DSS, NIST, etc.)
   - Audit results summary

4. Customer data protection:
   - How customer data is protected
   - Geographic residency (if applicable)
   - Data retention and deletion policies
   - Customer access and control options

5. Incident response:
   - Incident response procedures
   - Breach notification process and timeline
   - Past incidents and lessons learned (if applicable)
   - Security incident contact

6. Third-party and vendor management:
   - How we assess vendor security
   - Subprocessor lists and management
   - Data processing agreements

7. Security governance:
   - Roles and responsibilities
   - Board/executive oversight of security
   - Security training and awareness
   - Continuous improvement program

8. Audit and assessment results:
   - Recent audit findings and remediation
   - Customer audit accommodation policy
   - Penetration testing and vulnerability assessment results
   - Security assessment certification

9. Contact information:
   - Security and privacy officer contact
   - Incident reporting procedures
   - Assessment/audit requests

Include supporting documentation:
- Relevant certifications (ISO, SOC 2, etc.)
- Security data sheet or brief
- Customer DPA template
- Incident response SLA

Format: 10-15 pages, professional presentation with visuals.
```

**Expected Output:**
- Executive summary of security posture
- Detailed security capabilities overview
- Compliance and certification status
- Customer data protection specifics
- Incident response and breach notification SLA
- Governance and oversight summary
- Contact and escalation procedures
- Supporting certifications and assessments
- FAQ for customer security teams

**Limitations:**
- Report should not disclose sensitive security architecture details
- Customers may request additional details or audit rights
- Certifications/assessments require third-party validation

**Tips:**
- Lead with credibility: certifications, audit results, industry recognition
- Be transparent about past incidents and what you learned
- Provide clear data processing agreement and DPA
- Include security contact and escalation path (builds trust)

---

## 8. Risk-Based Remediation Roadmap

**Title:** Data Security Improvement Plan with Risk Prioritization

**Purpose:** Present prioritized remediation roadmap based on business risk and impact.

**When to Use:**
- Executive steering committee meeting
- Quarterly security planning
- Budget planning and allocation
- Multi-quarter strategic planning

**Exact Prompt:**
```
Create risk-based remediation roadmap:

Input: List of identified data security risks/gaps
Timeframe: 12-month roadmap with quarterly milestones

For each identified risk/gap:

1. Risk assessment:
   - Business impact if not remediated (revenue at risk, compliance failure?)
   - Likelihood of exploitation (threat intelligence, incident history)
   - Overall risk score: Critical / High / Medium / Low

2. Remediation approach:
   - What needs to be done to reduce this risk?
   - Options: policy change, technology investment, process improvement, training
   - Owner and sponsorship (who drives this)

3. Effort estimation:
   - Technical effort (dev, configuration, testing)
   - Business effort (process change, communication, training)
   - Timeline to complete
   - Resource requirements (people, budget, tools)

4. Success criteria:
   - How will we know this risk is remediated?
   - Metrics to track progress
   - Target completion date

5. Risk-reward prioritization:
   - High business impact + low effort = Phase 1 (quick wins)
   - High business impact + high effort = Phase 2 (strategic)
   - Medium/low impact = Phase 3 (nice-to-have or defer)

6. Quarterly roadmap:
   - Q1: High-priority items that address greatest risks
   - Q2: Follow-on remediation and strategic projects
   - Q3: Medium-priority initiatives
   - Q4: Catch-up and forward planning

7. Resource planning:
   - Total FTE required per quarter
   - Budget estimate for tools, consulting, training
   - Dependencies between initiatives (what must complete first?)

8. Risk trajectory:
   - Starting data risk score
   - Risk reduction per quarter
   - Target/desired state risk score

9. Governance:
   - Steering committee oversight
   - Monthly/quarterly progress reporting
   - Risk register updates
   - Escalation procedures if items slip

Include roadmap Gantt chart, risk heat map, and resource allocation chart.
```

**Expected Output:**
- Risk-prioritized remediation list (Critical/High/Medium/Low)
- Quarterly roadmap with milestones
- Resource and budget estimate
- Risk reduction projection
- Gantt chart for executive tracking
- Governance and oversight plan
- Contingency planning for risks/delays
- Communication plan for stakeholders

**Limitations:**
- Roadmap assumes stable priorities; business changes may reprioritize
- Some efforts may have hidden complexity
- Resource availability may constrain execution timeline

**Tips:**
- Lead with 3-5 quick wins (build momentum and credibility)
- Include strategic multi-quarter initiatives (show long-term thinking)
- Be realistic about resource constraints
- Track progress monthly and adjust as needed

---

## Tags and Patterns

- **[VALIDATED]** Prompts 1, 3, 4, 6: Core SC capabilities for executive synthesis and compliance reporting. Real SC implementations use these patterns.
- **[VALIDATED]** Prompts 2, 5, 7: Incident communication and posture reporting leverage SC's ability to contextualize data risk for different audiences.
- **[ASPIRATIONAL]** Prompts 8: Requires cross-system risk aggregation and roadmap integration. Recommend customizing to your org's risk framework.

All prompts assume SC has access to Purview audit, alert, and risk data. Adapt terminology and compliance references to match your regulatory scope.
