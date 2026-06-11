# User Personas: Lumen — Purview Data Security

## Persona 1: CSA / Security Consultant

**Role Description**

Customer Success Architects work across multiple customers, advising on data security architecture, Purview deployment, and incident response. They are the "translators" between Microsoft product capabilities and customer business requirements. They spend 40% of time in customer environments (workshops, architecture review, incident response), 30% on proposal and demo preparation, and 30% on internal collaboration and enablement.

**Day-to-Day Activities**

- Conduct security assessments and gap analyses across customer data security posture
- Design DLP, IRM, and DSPM strategies aligned with customer risk profiles and compliance needs
- Lead architecture workshops with customer leadership and technical teams
- Prepare and deliver interactive product demos (often with short notice)
- Respond to customer incidents by reviewing alert data, correlating findings, and coaching customer teams through investigation
- Write proposal documents, SOWs, and post-implementation reports
- Mentor customer teams on Purview best practices and alert investigation workflows

**Pain Points**

- Limited time to prepare for customer interactions; often asked for demos or workshops with 1-2 days notice
- Expected to be expert in 5+ Purview products (DLP, IRM, DSPM, MIP, Audit) despite limited hands-on time in each
- Struggle to quickly navigate between Purview consoles during live customer interactions; hesitation to "look lost" in front of executives
- Difficulty generating compelling demo scenarios and sample data without extensive setup time
- Customers expect post-incident reports within 24 hours; manual correlation and narrative building is time-consuming
- Knowledge about Purview behavior and security patterns is fragmented across CSA community; inconsistent advice across customers

**How They Use Security Copilot**

- **Workshop Prep**: Use pre-built Copilot prompts to generate demo scenarios, sample alert walkthroughs, and talking points on DLP false positives, IRM configuration, or DSPM findings
- **Live Investigation Walks**: During customer incidents, use Copilot-assisted investigation playbooks to quickly contextualize alerts (user behavior, historical patterns, data criticality) without appearing unprepared
- **Proposal Support**: Use Copilot to analyze customer audit logs or sample DLP alerts to generate evidence-based recommendations and ROI metrics
- **Executive Briefing**: Transform technical incident details into executive summaries suitable for customer C-suite communication
- **Rapid Knowledge Synthesis**: Ask Copilot to explain obscure Purview behaviors or reconcile conflicting customer questions without consulting internal forums

**Key Prompt Categories Needed**

- Investigation kick-start templates (DLP alert context enrichment, user activity correlation, data classification lookup)
- Demo scenario generators (realistic false positives, IRM permission misconfigurations, DSPM policy violations)
- Executive summary and talking-point generators
- Proposal analysis prompts (gap analysis synthesis, competitive positioning, ROI estimation)
- Troubleshooting guides (policy effectiveness, label coverage, IRM permission inheritance)

**Example Scenario**

CSA is asked Wednesday afternoon to deliver a Purview data security demo to a financial services customer on Friday. The customer is particularly concerned about DLP false positives in their trading-adjacent division. The CSA uses a Copilot "demo scenario generator" prompt to build a realistic incident (employee sending trading analysis to external advisor with IRM protection questions) that illustrates DLP detection, alert triage, IRM permission inheritance, and remediation. Copilot also generates talking points explaining why this scenario is common and how policy tuning reduces false positives. By Thursday evening, the CSA has a reproducible, polished demo with no manual lab setup needed.

**Priority Use Cases**

UC-DLP-001, UC-DLP-002, UC-IRM-001, UC-DSPM-001, UC-Audit-001, UC-Reporting-001, UC-Investigation-Triage, UC-Executive-Summary

---

## Persona 2: SOC / Security Analyst

**Role Description**

SOC analysts are the front-line responders to security alerts. They work in a 24/7 environment (or in-hours SOC) and are responsible for triaging alerts, determining whether they represent actual incidents, correlating findings across tools, and escalating confirmed incidents to incident response. They typically have broad security knowledge but limited expertise in any single product like Purview. They are measured on mean-time-to-triage (MTTT) and incident accuracy.

**Day-to-Day Activities**

- Review and triage DLP alerts, determining if they are legitimate policy violations or false positives
- Correlate DLP alerts with user behavior (historical activity, peer behavior, job role context)
- Investigate suspected data exfiltration by combining DLP signals with IRM permission logs and user audit trails
- Draft concise incident summaries and escalations for the IR team
- Respond to compliance requests by pulling evidence from DLP, IRM, and Audit
- Document incident timelines and remediation actions
- Participate in post-incident reviews and tuning recommendations for DLP policies

**Pain Points**

- Alert fatigue: DLP policies generate dozens to hundreds of alerts per day; triage is manual and time-consuming
- Context switching: Correlating signals requires jumping between DLP, IRM, Audit, and often Slack, email, and HR data
- Limited Purview expertise: Analysts may not understand DLP policy logic, IRM permission inheritance, or label definitions; fear of missing context
- Manual correlation: Building a coherent narrative from fragmented alerts requires time and forensic skill
- Reporting burden: Escalations and incident summaries are typed manually; inconsistent detail and quality
- No single source of truth: Alert context (user role, data sensitivity, peer baseline) requires multiple console lookups

**How They Use Security Copilot**

- **Alert Triage Acceleration**: Provide Copilot with a DLP alert + user profile context and ask it to flag obvious false positives (e.g., user sending internal regulatory docs to internal advisor) and prioritize genuine risk (e.g., user with no prior external-share activity suddenly exfiltrating customer PII)
- **Correlation**: When an analyst has data from multiple Purview surfaces (DLP alert + IRM deny event + user never exported before), ask Copilot to correlate and identify threat patterns
- **Investigation Narrative**: Provide Copilot with alert timeline and supporting logs; ask it to synthesize a coherent investigation summary suitable for escalation
- **Policy Tuning Input**: Ask Copilot to analyze false-positive patterns in DLP alerts and recommend policy refinements
- **Compliance Response**: When asked to find all instances of a specific data type being shared, ask Copilot to help construct the query and summarize findings

**Key Prompt Categories Needed**

- Alert triage decision trees (DLP false positive heuristics, priority scoring, escalation criteria)
- Context enrichment templates (user behavior baseline, peer activity comparison, job role analysis)
- Investigation summary generators
- Correlation playbooks (DLP + IRM + Audit signal synthesis)
- Policy tuning analysis prompts
- Compliance evidence gathering and summarization

**Example Scenario**

Analyst receives 15 DLP alerts for "Confidential" labeled files sent to external recipients in a 2-hour window. Manually reviewing each alert would take 30-45 minutes. Instead, they use a Copilot "alert triage" prompt, providing the alert list and a quick lookup of the users' roles and historical behavior. Copilot flags 10 alerts as likely false positives (the users are project managers sending statements of work to partner firms, which is common for their role), prioritizes 5 alerts for further investigation (one user has never sent external before; another is sending to a personal Gmail account), and recommends escalation of 2 alerts to IR. The analyst completes triage in 15 minutes instead of an hour and provides context-rich escalations.

**Priority Use Cases**

UC-DLP-001, UC-DLP-002, UC-DLP-003, UC-IRM-002, UC-Audit-002, UC-Investigation-Triage, UC-Investigation-Summary, UC-Policy-Tuning

---

## Persona 3: Compliance Analyst

**Role Description**

Compliance analysts ensure the organization meets regulatory and policy requirements. They work with data governance, legal, and HR teams to maintain compliance posture, respond to audit requests, track remediation of findings, and report status to leadership. They use Purview primarily to assess policy effectiveness, gather evidence for audits, and demonstrate control maturity to regulators.

**Day-to-Day Activities**

- Assess data security posture against regulatory requirements (HIPAA, PCI, GDPR, SOX, etc.)
- Analyze DLP policy coverage and label distribution across sensitive data types
- Respond to regulatory requests and audit findings by gathering evidence from Purview
- Review and approve new DLP policies and IRM configurations to ensure compliance alignment
- Track remediation of data security findings and policy violations
- Prepare compliance status reports and board-level risk summaries
- Coordinate with data owners to remediate policy gaps (e.g., insufficient label coverage)

**Pain Points**

- Manual data gathering: Compiling evidence from multiple Purview consoles is time-consuming and error-prone
- Compliance pressure: Regulatory deadlines are non-negotiable; audit responses take weeks of prep
- Cross-product visibility gaps: Policy coverage across DLP + IRM + MIP + DSPM is opaque; difficult to assess true protection
- Evidence presentation: Auditors want clear, searchable documentation; manual compilation creates inconsistency
- Label sprawl and coverage gaps: Unknown whether sensitive data is properly classified and protected
- Knowledge gaps: Understanding the nuances of DLP policy logic, IRM permission inheritance, and compliance-relevant data attributes requires significant learning

**How They Use Security Copilot**

- **Compliance Posture Assessment**: Provide Copilot with a sample of DLP alerts, label distribution data, and policy configuration; ask it to assess coverage gaps and compliance risk
- **Policy Gap Analysis**: Ask Copilot to review current DLP policies and recommend additional rules needed to address regulatory requirements
- **Regulatory Reporting**: Provide Copilot with incident data, remediation actions, and policy metrics; ask it to synthesize a compliance status report suitable for audit or board presentation
- **Label Coverage Analysis**: Ask Copilot to analyze file classification data and identify data types with insufficient label coverage
- **Audit Response Support**: When faced with an audit finding (e.g., "demonstrate that all customer PII is protected"), ask Copilot to help construct the evidence narrative and supporting queries
- **Risk Assessment Briefings**: Ask Copilot to synthesize data security posture into a risk heat map suitable for executive review

**Key Prompt Categories Needed**

- Compliance posture assessment templates (coverage analysis, gap identification, risk rating)
- Policy gap analysis prompts (regulatory alignment, control maturity assessment)
- Label coverage and classification analysis
- Regulatory reporting and audit response generators
- Evidence synthesis and documentation templates
- Risk heat map and trend analysis generators

**Example Scenario**

Compliance analyst is preparing for an annual SOC 2 audit. Auditors will ask for evidence that PII and PHI are protected by DLP policies and that unauthorized sharing is blocked or detected. The analyst uses a Copilot "compliance posture assessment" prompt, providing current DLP policy config, label distribution data, and historical alert volume. Copilot identifies that while most customer PII is labeled "Confidential," a significant portion of employee health data (PHI) is not labeled consistently; DLP policies cover 70% of required data types but miss some regulatory data elements. Copilot drafts a compliance report highlighting gaps, recommends additional policies, and suggests a remediation timeline. The analyst now has audit-ready documentation instead of spending a week assembling evidence manually.

**Priority Use Cases**

UC-DLP-001, UC-DLP-002, UC-DSPM-001, UC-MIP-001, UC-Audit-001, UC-Reporting-Compliance, UC-Risk-Assessment, UC-Investigation-Summary

---

## Persona 4: Data Security Admin

**Role Description**

Data security admins are responsible for the day-to-day operation and optimization of Purview data security controls. They configure DLP policies, manage sensitivity labels, tune IRM templates, oversee DSPM monitoring, and respond to operational alerts. They are deeply familiar with Purview mechanics and are measured on alert accuracy, policy effectiveness, and operational stability. They often serve as the subject-matter expert for their organization's data security tooling.

**Day-to-Day Activities**

- Monitor and tune DLP policies to reduce false positives while maintaining detection effectiveness
- Manage sensitivity label taxonomy and rollout to end users
- Configure IRM templates and troubleshoot permission inheritance issues
- Review DSPM alerts for misconfigurations, oversharing, and control gaps
- Analyze alert patterns to identify policy effectiveness and opportunities for improvement
- Prepare documentation for policy changes and communicate changes to business stakeholders
- Test new Purview features and capabilities, evaluate adoption and impact

**Pain Points**

- Policy sprawl: Over time, DLP policies accumulate and become difficult to maintain, with overlaps and contradictions
- False positive tuning: Reducing false positives without sacrificing detection is a delicate balancing act; poor tuning decisions can be expensive to reverse
- Configuration complexity: Understanding how DLP policy rules interact, how IRM permissions cascade, and how label inheritance works requires deep product knowledge
- Impact assessment: Before deploying a new policy, difficulty predicting alert volume, false positive rates, and business impact
- Label adoption: Rolling out and maintaining a consistent label taxonomy across the organization is challenging; coverage is often incomplete
- Knowledge isolation: Best practices and troubleshooting knowledge is often undocumented and held by one person; organizational knowledge risk

**How They Use Security Copilot**

- **False Positive Analysis**: When DLP policies are generating high false positive rates, use Copilot to analyze alert patterns and recommend specific policy refinements (e.g., adding business process exceptions, refining sensitive data detection)
- **Policy Tuning Recommendations**: Provide Copilot with alert data, policy configuration, and false positive feedback; ask it to recommend adjustments
- **Configuration Review**: Ask Copilot to review current DLP policies or IRM configurations and identify potential issues, overlaps, or improvements
- **Impact Prediction**: Before deploying a new policy, ask Copilot to estimate alert volume and false positive rate based on similar policies and organizational baseline
- **Label Coverage Analysis**: Ask Copilot to assess label adoption and recommend strategies to improve coverage
- **Troubleshooting Guides**: Ask Copilot to explain why a policy or configuration is behaving unexpectedly (e.g., why IRM is allowing sharing that should be blocked)

**Key Prompt Categories Needed**

- False positive analysis and policy tuning templates
- Policy effectiveness assessment prompts
- Configuration review and troubleshooting guides
- Impact prediction and estimation (alert volume, FP rate, performance)
- Label coverage and adoption analysis
- Policy overlap and consolidation analysis

**Example Scenario**

Data security admin is managing a DLP policy designed to protect "Trade Secrets." The policy has been generating 20-30 alerts per day, but investigation shows 70% are false positives (legitimate internal emails with terms commonly used in contract discussions). Before disabling the policy, the admin uses a Copilot "false positive analysis" prompt, providing the last 30 days of alerts, the policy configuration, and false positive feedback. Copilot identifies that the regex patterns are too broad and recommends specific refinements: narrowing the pattern to match full confidentiality statements, adding an exception for internal email, and refining the sensitive data indicators. The admin implements the recommendations, reducing false positives to 10% while maintaining detection of actual trade secrets. The investigation that would have taken 4 hours is completed in 1 hour with clear, actionable recommendations.

**Priority Use Cases**

UC-DLP-002, UC-DLP-003, UC-IRM-001, UC-IRM-002, UC-DSPM-001, UC-Policy-Tuning, UC-Configuration-Review, UC-Operational-Readiness

---

## Persona 5: Security Leadership / CISO

**Role Description**

CISOs and security leaders are responsible for enterprise-wide security risk management, board and stakeholder reporting, security investment decisions, and incident response oversight. They operate at a high level of abstraction and are primarily interested in risk metrics, trends, compliance status, and strategic implications. They have limited time for technical details and expect clear, actionable summaries.

**Day-to-Day Activities**

- Oversee incident response for high-profile breaches or policy violations
- Prepare quarterly and annual security posture reports for the board and executives
- Assess security investments and justify budget to leadership
- Set strategic priorities for data security based on risk assessment
- Respond to regulatory inquiries and audit findings
- Track security metrics and KPIs (MTTR, SLA compliance, policy effectiveness, etc.)
- Communicate security risks and controls to business leaders in non-technical language

**Pain Points**

- Data overload: Too much raw security data, not enough contextualized insight. Receives hundreds of alerts but needs to understand the top 10 risks
- Time constraints: Cannot spend time digging into technical details; needs rapid summaries and clear implications
- Reporting burden: Building quarterly security reports from fragmented data sources is time-consuming and pulls focus from strategic work
- Justification pressure: Security investments must be justified with clear business impact; needs data-driven arguments
- Trend visibility: Difficult to identify emerging data security risks or validate whether controls are becoming more or less effective
- Technical translation: Must explain technical control effectiveness and policy violations to business leaders who lack security expertise

**How They Use Security Copilot**

- **Executive Summaries**: Provide Copilot with incident details (DLP alerts, user behavior, remediation); ask for a board-ready summary highlighting risk, business impact, and response
- **Trend Analysis**: Provide Copilot with quarterly alert and incident data; ask for risk heat map and trend analysis suitable for executive review
- **Risk Posture Overview**: Ask Copilot to synthesize current data security posture (policy coverage, alert trends, compliance status) into a strategic risk assessment
- **Post-Incident Briefings**: After a significant incident, ask Copilot to analyze root cause, impact, and recommendations in a format suitable for C-suite and board review
- **Investment Justification**: Ask Copilot to analyze data security metrics and recommend investments with clear ROI projections
- **Regulatory Readiness**: When facing an audit or regulatory requirement, ask Copilot to assess readiness and identify gaps or risks

**Key Prompt Categories Needed**

- Executive summary and board briefing templates
- Risk posture assessment and heat map generators
- Trend analysis and KPI synthesis
- Post-incident review and strategic recommendation generators
- Investment justification and ROI analysis
- Regulatory readiness and risk assessment

**Example Scenario**

CISO is preparing for a board review after a significant data exfiltration incident. Board members will ask for the scope of the incident, whether controls worked as expected, and what is being done to prevent recurrence. The incident involved DLP alerts, IRM permission review, user behavior investigation, and policy tuning. Instead of spending a day gathering and analyzing data, the CISO uses a Copilot "post-incident executive brief" prompt, providing incident timeline, alert data, investigation findings, and remediation actions. Copilot synthesizes this into a 2-page board brief that explains what happened in business terms, quantifies the impact, highlights control strengths and weaknesses, and recommends specific investments (e.g., label coverage improvement, IRM permission review process). The CISO has a polished, data-driven brief ready for board discussion instead of a day of manual work.

**Priority Use Cases**

UC-Reporting-Executive, UC-Reporting-Compliance, UC-Risk-Assessment, UC-Investigation-Summary, UC-Policy-Effectiveness, UC-Incident-Response-Oversight

