# Use Case Catalog: Lumen — Purview Data Security

**Framework Version:** 1.1
**Last Updated:** April 2026
**Status:** Production Ready
**Coverage:** 22 validated use cases across DLP, IRM, DSPM, Investigation, MIP, Reporting, Operations

---

## Overview

This catalog contains 22 production-ready use cases for Security Copilot applied to Microsoft Purview Data Security. Each use case has been designed with realistic customer scenarios, tested prompt patterns, and honest assessments of capabilities and limitations.

Use cases are organized by functional area: DLP (4), IRM (4), DSPM (2), Investigation (3), MIP (2), Reporting (3), Operations (2).

All prompts leverage only the 6 validated Purview plugin capabilities and 4 SC agents. No fictional capabilities.

---

## DLP (Data Loss Prevention)

### UC-DLP-001: DLP Alert Triage and Prioritization

**UC ID:** UC-DLP-001
**Title:** DLP Alert Triage and Prioritization
**Objective:** Rapidly assess incoming DLP alerts to prioritize investigation effort, distinguish genuine incidents from false positives, and route alerts to the correct team.
**Target Persona:** SOC Analyst, Data Security Admin

**SC Experience:** Embedded (DLP alert pane in Purview)

**SC System Capabilities Used:**
- Triage Purview Alerts (top/recent DLP alerts with risk ranking)
- Summarize Purview Alert (quick alert comprehension)
- Get Data Risk Summary (sensitive content exposure context)

**Key Prompts:**

1. "We received 47 DLP alerts in the last 4 hours. Triage them by risk level (Critical, High, Medium, Low). Which 5 should I investigate first? Include the policy triggered, user, and one-line reason for priority."

2. "This user has triggered the 'Financial Data' DLP policy 12 times in the past week. Is this an insider threat pattern or normal business activity? What additional context do I need to know?"

3. "Compare these three DLP alerts: [Alert A: Sales user emailing customer prospect list] [Alert B: Developer copying source code to personal cloud storage] [Alert C: Analyst downloading compliance report]. Rank by investigation priority and explain your reasoning."

**Expected Output:** Prioritized alert queue with risk assessment, suggested investigation path, and recommended action (escalate, investigate, monitor, resolve as non-incident). SC surfaces patterns (repeated violations by same user) and highlights anomalies.

**Limitations:** SC cannot determine if content was legitimately needed without human context. May over-prioritize based on rule name alone. Alert volume spikes may reduce analysis quality. Requires human review before disciplinary action.

**Human Validation Points:**
1. Before escalating any alert to investigation or compliance team
2. Before concluding a pattern indicates insider threat
3. Before implementing alerts as automated blocks (may be legitimate workflows)

**Customer Value:** SOC analysts handle hundreds of daily alerts. SC can reduce triage time by 40-60% by automatically assessing severity, surfacing context, and flagging high-risk patterns. This frees analysts to focus on true incidents rather than alert fatigue.

---

### UC-DLP-002: DLP Policy Match Investigation

**UC ID:** UC-DLP-002
**Title:** DLP Policy Match Investigation
**Objective:** Deep-dive into why a specific DLP policy rule matched an alert, understand what content triggered the rule, assess false positive risk, and determine if the policy is working as intended.
**Target Persona:** Data Security Admin, Compliance Analyst, CSA

**SC Experience:** Embedded or Standalone

**SC System Capabilities Used:**
- Zoom Into Purview Data Risk (data risk attributes, policy matches over time)
- Summarize Purview Alert (alert key facts)
- Get Data Risk Summary (MIP label and sensitive content context)

**Key Prompts:**

1. "This user's email was blocked by the 'Credit Card Numbers' rule, but the content is our monthly billing report with masked PAN numbers. Why did the policy match? Is this a false positive?"

2. "Our 'Healthcare Data' DLP policy is triggering on patient names + medical terms, but it's also catching employee benefits emails and health insurance FAQs. Help me refine the rule to reduce false positives while protecting real PHI."

3. "Explain how the 'High Volume Data Transfer' DLP rule matched this alert. What exact content or patterns triggered it? Would this be legitimate for a data analyst?"

**Expected Output:** Explanation of which sensitive information type (SIT) or pattern matched the rule. Assessment of whether the match indicates a true incident or false positive. Suggestions for rule refinement if over-triggering. Risk rating. Should include whether the user's role and workflow context make this activity expected.

**Limitations:** SC may not have visibility into custom SITs if not well documented. Cannot determine intent (accidental vs. malicious) without context. False positive assessment requires understanding legitimate organizational data flows. Regex-based rules harder to explain than built-in SITs.

**Human Validation Points:**
1. Before concluding a match is definitely a false positive
2. Before refining policy rules—test refinements in audit/simulation mode first
3. If considering whitelisting a user or content type, validate with data owner and compliance team

**Customer Value:** DLP false positives are a major source of team frustration and user friction. A security admin can spend hours investigating policy matches. SC cuts investigation time dramatically by explaining matches in business context and flagging obvious false positives. This reduces burnout and improves policy tuning accuracy.

---

### UC-DLP-003: DLP False Positive Pattern Analysis

**UC ID:** UC-DLP-003
**Title:** DLP False Positive Pattern Analysis
**Objective:** Systematically analyze DLP alert patterns to identify widespread false positives, understand root causes, and provide evidence-based recommendations for policy tuning.
**Target Persona:** Data Security Admin, Security Leadership

**SC Experience:** Standalone (bulk analysis)

**SC System Capabilities Used:**
- Triage Purview Alerts (categorize by risk, false positive markers)
- Zoom Into Purview Data Risk (historical alert patterns, policy match trends)
- Get Data Risk Summary (sensitive content types triggering rules)

**Key Prompts:**

1. "I'm seeing 600 DLP alerts per week. 85% are marked 'Resolved - Non-Incident.' What patterns indicate that specific rules are generating excessive false positives? Which rules should I review first?"

2. "Our 'Financial Data' policy is triggering heavily on emails from the Finance department sharing budget spreadsheets with each other. This appears to be legitimate business activity. Based on alert patterns, what's the evidence for and against disabling or narrowing this rule?"

3. "Analyze the false positive rate by rule, by user group, and by content type. Which combination is causing the most friction? What's a quick win we can fix?"

**Expected Output:** Breakdown of false positive rate by rule and user group. Analysis of common characteristics of false positives vs. true incidents. Prioritized list of rules to review/tune. Specific refinement recommendations (e.g., "limit rule to emails containing both 'Account Number' AND 'Routing Number' within 50 characters"). Quantify impact.

**Limitations:** Requires sufficient historical data (at least 30 days). "Resolved - Non-Incident" must be accurately recorded. SC may not fully understand legitimate organizational workflows without detailed context. Over-aggressive tuning can create policy gaps.

**Human Validation Points:**
1. Review and approve any recommended policy rule changes before deployment
2. Test rule changes in audit/simulation mode on historical alerts before enforcement
3. Validate with data owners that refined rules still protect sensitive data

**Customer Value:** False positive overload is a leading cause of DLP program failure. Teams either stop responding to alerts or over-block legitimate business, frustrating users and slowing collaboration. SC helps teams tune their DLP program to be both effective and usable. Essential for long-term DLP success.

---

### UC-DLP-004: DLP Policy Coverage Gap Analysis

**UC ID:** UC-DLP-004
**Title:** DLP Policy Coverage Gap Analysis
**Objective:** Assess DLP policy coverage and recommend new rules or adjustments based on organizational data sensitivity, risk profile, and recent incidents.
**Target Persona:** Data Security Admin, Compliance Analyst, CISO

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (current data risk coverage and gaps)
- Zoom Into Purview Data Risk (data types protected and unprotected)
- Triage Purview Alerts (past incidents indicating coverage gaps)

**Key Prompts:**

1. "Review our current DLP policies and our data classification. We have significant PII but no specific rule for email addresses + phone numbers in combination. What new rules should we add to close coverage gaps? Prioritize by risk and false positive likelihood."

2. "We had an incident where an employee exfiltrated customer data via personal cloud storage. Our current DLP only monitors email and Teams. What policies should we add to detect and prevent data loss to unmanaged cloud storage?"

3. "Based on our industry (healthcare) and compliance requirements, audit our DLP policy suite. Are we missing rules for PHI, drug trial data, or research data? What's our coverage maturity: Basic, Intermediate, Advanced, or Mature?"

**Expected Output:** Gap analysis of current DLP coverage. Prioritized list of recommended new rules (with business case and false positive risk assessment). Mapping of rules to compliance requirements. Maturity assessment of overall policy suite. Phased implementation roadmap.

**Limitations:** Recommendations only as good as input data about incidents and organizational data types. If organization hasn't classified most of its data, SC cannot make data-driven recommendations. Implementation effort often underestimated.

**Human Validation Points:**
1. Validate gap analysis with data owners and compliance team
2. Prioritize recommendations based on organizational risk tolerance and compliance deadlines
3. Test new rules in audit mode; review false positive rate with sample data
4. Align recommendations with broader data governance initiatives

**Customer Value:** Many organizations have reactive DLP posture—add rules only after incidents occur. SC enables proactive approach: systematically assess coverage, identify gaps before breaches, and build defensible policy framework. For regulated industries, this translates to better audit readiness and reduced breach risk.

---

## IRM (Insider Risk Management)

### UC-IRM-001: Insider Risk Alert Triage

**UC ID:** UC-IRM-001
**Title:** Insider Risk Alert Triage
**Objective:** Rapidly assess incoming IRM alerts to identify which represent genuine insider threat risk, which are false positives, and determine investigation priority and next steps.
**Target Persona:** SOC Analyst, IRM Analyst

**SC Experience:** Embedded (IRM case pane in Purview)

**SC System Capabilities Used:**
- Get User Risk Summary (insider risk profile, risk indicators, risk level)
- Summarize Purview Alert (quick alert comprehension)
- Zoom Into Purview User Risk (activities, exfiltration sequences, anomalies)

**Key Prompts:**

1. "Summarize this IRM alert for me. What are the top 3 risk indicators? Should I escalate to investigation or close it as benign activity?"

2. "This user is flagged for 'unusual download volume from SharePoint.' Their role is data analyst; they regularly download reports for analysis. Is this a real threat or expected behavior for their job?"

3. "Compare these two users' risk profiles. User A: elevated data access activity, policy violations, terminated next month. User B: data exfiltration alerts, recent cloud app uploads, still actively employed. Which poses higher risk? Why?"

**Expected Output:** Assessment of whether alert represents genuine insider threat risk. Risk indicator breakdown. Investigation priority. Recommended next steps (escalate to investigation, monitor, close as benign). Should contextualize user role and recent employment changes.

**Limitations:** SC cannot determine if data access or download is authorized without business context. Cannot distinguish between legitimate departing employee data gathering and malicious exfiltration. May over-weight activity anomalies without understanding normal baseline for user's role.

**Human Validation Points:**
1. Before escalating to security investigation or HR
2. Before taking any employment action based on IRM alerts
3. Validate with manager or data owner that activity is anomalous for the user's role

**Customer Value:** IRM teams receive dozens of alerts daily. Most are benign; some are critical. SC can accelerate triage by ranking risk indicators, providing context, and flagging critical patterns. This enables investigators to focus on real threats rather than false alarms.

---

### UC-IRM-002: User Risk Timeline and Behavior Analysis

**UC ID:** UC-IRM-002
**Title:** User Risk Timeline and Behavior Analysis
**Objective:** Reconstruct a user's timeline of activities and behaviors to establish context for insider threat investigation, distinguish intentional exfiltration from legitimate work, and identify escalation triggers.
**Target Persona:** Investigation Analyst, IRM Case Owner

**SC Experience:** Embedded or Standalone

**SC System Capabilities Used:**
- Zoom Into Purview User Risk (activities, exfiltration sequences, anomalies)
- Get User Risk Summary (risk indicators over time)
- Summarize Purview Alert (specific activity context)

**Key Prompts:**

1. "Reconstruct this user's timeline for the past 30 days. When did risky activities spike? Were there policy violations before the exfiltration alert? Does the timeline suggest escalation or one-off anomaly?"

2. "This user's IRM risk profile shows elevated data access and cloud app uploads. In their department, is this pattern consistent with their job function? What activities would you expect vs. what's being flagged?"

3. "The user was placed on a performance improvement plan (PIP) three weeks ago. Did their risk activity increase after the PIP notification? Is there a correlation with employment status?"

**Expected Output:** Chronological timeline of user's flagged activities. Assessment of whether pattern suggests intentional data theft or legitimate work acceleration. Identification of key escalation moments. Recommended investigation path based on timeline context.

**Limitations:** IRM alert timeline may have gaps (especially early in user's profile). SC cannot access detailed file contents (only metadata). Cannot determine if cloud app uploads were authorized. Cannot see internal messaging or user intent.

**Human Validation Points:**
1. Validate timeline against manager and HR records
2. Confirm that flagged activities are actually anomalous for this user's role
3. Before concluding intentional exfiltration, verify no legitimate business reason exists

**Customer Value:** Insider threat investigations often hinge on understanding context and intent. SC accelerates context gathering by building timeline, correlating activities, and flagging escalation moments. This enables investigators to distinguish true threats from false alarms faster.

---

### UC-IRM-003: Cross-Alert Correlation (IRM + DLP + Audit)

**UC ID:** UC-IRM-003
**Title:** Cross-Alert Correlation (Insider Risk + Data Loss Prevention)
**Objective:** Correlate signals from IRM alerts, DLP matches, and audit logs to establish whether a user's risky behaviors indicate coordinated data theft or isolated incidents.
**Target Persona:** Investigation Analyst, Threat Analyst, CISO

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get User Risk Summary (IRM risk indicators)
- Get Data Risk Summary (DLP matches for the user)
- Zoom Into Purview User Risk (detailed activity sequences)
- Zoom Into Purview Data Risk (sensitive data matched)

**Key Prompts:**

1. "This user is flagged by IRM for elevated data access. Their DLP alerts show they attempted to email a customer contact list to a personal Gmail account. Are these independent incidents or part of a coordinated exfiltration? What's the evidence?"

2. "Correlate this user's IRM alerts, DLP matches, and Exchange audit logs. Timeline: Week 1 - elevated data access; Week 2 - DLP block on financial data email; Week 3 - high volume download from SharePoint; Week 4 - upload to personal cloud. Is this escalating threat or unrelated activities?"

3. "User A has multiple IRM exfiltration alerts but no DLP matches. User B has few IRM alerts but multiple DLP matches for sensitive data. Which poses greater insider threat risk? Why?"

**Expected Output:** Assessment of whether signals form coherent threat narrative or are isolated incidents. Timeline showing escalation or de-escalation. Evidence-based conclusion. Recommended investigation priority and next steps. Should explicitly state confidence level and data gaps.

**Limitations:** SC depends on all three signal sources being available and current. Data freshness varies (IRM real-time, DLP 15-30 min, audit logs 24-48 hr). Cannot access file contents, only metadata. Correlation alone cannot prove intent. May miss context (e.g., manager authorized the data gathering).

**Human Validation Points:**
1. Before concluding coordinated data theft, validate business context with manager
2. Confirm that all signals (IRM, DLP, audit) are accurate and not false positives
3. If recommending employment action, obtain legal and HR approval

**Customer Value:** Insider threat investigations require connecting signals from multiple systems. SC dramatically reduces investigation time by automatically correlating signals, establishing narrative, and highlighting escalation patterns. This enables faster, higher-confidence threat determination.

---

### UC-IRM-004: Risk Score Interpretation and Escalation Decision

**UC ID:** UC-IRM-004
**Title:** Risk Score Interpretation and Escalation Decision
**Objective:** Interpret Purview IRM risk scores, understand what factors drive a user's risk level, and make defensible decisions about case escalation, monitoring, or closure.
**Target Persona:** IRM Analyst, Investigation Manager, Compliance Officer

**SC Experience:** Embedded or Standalone

**SC System Capabilities Used:**
- Get User Risk Summary (risk score and indicator breakdown)
- Zoom Into Purview User Risk (risk score drivers and anomalies)
- Summarize Purview Alert (case summary)

**Key Prompts:**

1. "This user's IRM risk score is 7/10 (High). The score is driven by 'frequent data downloads' and 'policy violations.' Are these risk indicators serious enough to escalate to formal investigation, or should we monitor and let alerts accumulate?"

2. "User A has risk score 4/10 (Medium); User B has 6/10 (High). Both work in Finance. User A downloads large reports daily (expected for role); User B has exfiltration alert. Which should be escalated to investigation and why?"

3. "Explain how Purview's risk scoring works. What risk indicators matter most? If I want to deprioritize 'frequent data access' (expected for Data Scientists) and prioritize 'policy violations,' how should I tune IRM detection rules?"

**Expected Output:** Explanation of risk score components and their relative weight. Assessment of whether high risk score warrants escalation or is driven by benign activities common in the user's role. Clear recommendation (escalate, monitor, close) with justification.

**Limitations:** IRM risk score is a statistical model; may not reflect true insider threat risk in all contexts. Different roles have different baseline risk profiles. Cannot factor in business context without analyst input. Risk score may lag actual threat by hours or days.

**Human Validation Points:**
1. Before escalating to investigation, validate risk indicators are anomalous for user's role
2. If risk score is driven by expected activities (data access for analysts), adjust monitoring thresholds or detection rules
3. Document rationale for escalation or closure decisions for compliance audit trail

**Customer Value:** IRM risk scores can be opaque. SC provides transparency by explaining score drivers, contextualizing them against user's role, and supporting escalation decisions with evidence. This enables faster, more defensible decisions and improves investigation quality.

---

## DSPM (Data Security Posture Management)

### UC-DSPM-001: Data Exposure and Sensitivity Assessment

**UC ID:** UC-DSPM-001
**Title:** Data Exposure and Sensitivity Assessment
**Objective:** Analyze DSPM findings to understand what sensitive data is exposed in which locations, assess risk from exposure, and prioritize remediation efforts.
**Target Persona:** Data Security Admin, Data Steward, Compliance Officer

**SC Experience:** Standalone or Embedded (Purview DSPM pane)

**SC System Capabilities Used:**
- Get Data Risk Summary (sensitive data locations, exposure status, remediation progress)
- Zoom Into Purview Data Risk (detailed exposure attributes: unencrypted, unclassified, excessive access)
- DSPM Posture Agent (natural language queries: "show me all databases with unencrypted PII")

**Key Prompts:**

1. "Our DSPM scan found 500 databases with unencrypted customer PII. Which should we prioritize for remediation? Consider: data sensitivity, number of records, exposure duration, remediation effort."

2. "Analyze our DSPM posture. What percentage of sensitive data is properly classified, encrypted, and access-controlled? What are the biggest gaps? Which gap creates the most risk?"

3. "Query: Show me all SharePoint sites with unclassified financial data accessible to >50 users. Rank by remediation priority. What's the recommended remediation path for each?"

**Expected Output:** Assessment of what sensitive data is exposed and in what locations. Risk ranking of exposures. Prioritized remediation roadmap with effort estimates. Should quantify current posture and recommend targets.

**Limitations:** DSPM scans are periodic (not real-time). Scan results may lag by days or weeks. Cannot determine if exposure is authorized (some users legitimately need access). Cannot remediate directly; all actions are recommendations.

**Human Validation Points:**
1. Validate that flagged data is actually sensitive (may be test/demo data)
2. Confirm that user access to flagged data is not authorized or necessary
3. Before restricting access or encrypting data, validate with data owner and application teams

**Customer Value:** Most organizations lack visibility into where sensitive data actually resides and who can access it. DSPM + SC closes this gap by identifying exposures, assessing risk, and recommending fixes. This enables systematic posture improvement rather than reactive response to breaches.

---

### UC-DSPM-002: Sensitive Data Classification and Labeling Gap Analysis

**UC ID:** UC-DSPM-002
**Title:** Sensitive Data Classification and Labeling Gap Analysis
**Objective:** Identify data that should be classified and labeled but isn't, understand barriers to classification coverage, and establish roadmap for improving data governance maturity.
**Target Persona:** Data Steward, Compliance Officer, Chief Data Officer

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (classified vs. unclassified sensitive data)
- Zoom Into Purview Data Risk (data locations lacking sensitivity labels)
- DSPM Posture Agent (natural language queries: "show me all files containing credit card numbers without 'Confidential' label")

**Key Prompts:**

1. "Our DSPM findings show 2000 files with detected PII but no sensitivity label applied. What's preventing classification? Are these scattered across the organization or concentrated in specific teams? What's the fastest path to increase label coverage?"

2. "We have 5 sensitivity labels defined, but only 20% of sensitive data is labeled. Analyze the gap. Is it a training issue? A workflow issue? A technical limitation of our labeling tools? Recommend improvements."

3. "Compare classification coverage by department. Finance is 80% labeled; Sales is 20% labeled. Why the gap? What's unique about Sales that makes labeling harder? What can we learn from Finance?"

**Expected Output:** Quantified gap between sensitive data discovered and labeled. Analysis of why gap exists (lack of awareness, tool friction, workflow complexity). Prioritized roadmap to close gap. Should identify quick wins and long-term structural changes.

**Limitations:** Classification decisions depend on business context. SC cannot determine if gap is due to training, tool friction, or intentional decision. Cannot remediate directly. Closing gap requires sustained effort and change management.

**Human Validation Points:**
1. Involve data stewards and business leaders in prioritizing which data to classify first
2. Validate that classification recommendations align with organizational data governance policies
3. Get buy-in from department leaders before rolling out labeling requirements

**Customer Value:** Data classification and labeling are foundational to modern data security, but most organizations struggle with coverage. SC enables data leaders to understand barriers, prioritize improvement efforts, and track progress. By improving classification maturity, organizations build better visibility and more effective DLP/IRM.

---

## Investigation (DLP + IRM + Audit Correlation)

### UC-INV-001: Data Leakage Investigation and Containment

**UC ID:** UC-INV-001
**Title:** Data Leakage Investigation and Containment
**Objective:** Investigate a confirmed or suspected data leakage incident by correlating DLP alerts, IRM cases, audit logs, and DSPM findings to establish what data was lost, to whom, when, and how to contain it.
**Target Persona:** Incident Responder, Investigation Analyst, Incident Commander

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (what sensitive data is at risk)
- Get User Risk Summary (user involvement and insider risk context)
- Zoom Into Purview Data Risk (detailed data exposure and movement)
- Zoom Into Purview User Risk (user's exfiltration activities and timeline)

**Key Prompts:**

1. "We suspect data exfiltration. User X triggered DLP alerts for email; also has IRM alerts for unusual cloud uploads. Reconstruct: What data was accessed? When? Which systems? Who was it shared with? Is it confirmed exfiltration or suspicious access without actual loss?"

2. "Customer reported we lost their contract data. Search Purview DLP, IRM, and audit logs. Which users accessed the contract? Did any try to share it externally? What's the most likely exfiltration path (email, cloud upload, USB)? What data is still at risk?"

3. "Confirmed data loss: customer list with 5000 contacts was shared to external Gmail. Timeline: when was list accessed, modified, shared? Which users involved? Did others access the list before/after? What's the scope of breach?"

**Expected Output:** Forensic timeline of data movement from discovery to loss. Evidence of which users accessed and exfiltrated data. Identification of what data was lost (fields, records, sensitivity). Time window of exposure. Likely exfiltration method. Recommendations for containment. Should quantify breach scope.

**Limitations:** Investigation depends on all data sources being enabled (DLP, IRM, audit). Audit logs lag 24-48 hours. Cannot access file contents (only metadata). Cannot determine if external recipient has made copies or onward sharing. May miss exfiltration via unmonitored methods.

**Human Validation Points:**
1. Validate timeline against system timestamps and user interviews
2. Confirm external recipient details; initiate breach notification if warranted
3. If incident involves potential crime, coordinate with legal and law enforcement
4. Document all investigation findings for incident report and forensic record

**Customer Value:** Data exfiltration investigations are complex, time-consuming, and high-stakes. SC accelerates investigation by automatically correlating signals, establishing timeline, identifying users and data involved, and recommending containment. This enables faster response, more complete evidence gathering, and better incident documentation.

---

### UC-INV-002: DLP False Positive vs. True Incident Determination

**UC ID:** UC-INV-002
**Title:** DLP False Positive vs. True Incident Determination
**Objective:** Investigate ambiguous DLP alerts where it's unclear whether the match indicates real data loss risk or false positive, using contextual data from IRM, audit logs, and business process knowledge.
**Target Persona:** Investigation Analyst, Data Security Admin

**SC Experience:** Embedded or Standalone

**SC System Capabilities Used:**
- Summarize Purview Alert (alert context)
- Zoom Into Purview Data Risk (data sensitivity and exposure)
- Get User Risk Summary (user's risk profile and history)
- Zoom Into Purview User Risk (user's recent activities)

**Key Prompts:**

1. "DLP alert: User shared 'Financial Summary' file to external email. File name contains 'financial data,' triggering our Finance DLP rule. But it's a PowerPoint slide deck, not detailed data. Is this a false positive or genuine incident? What additional info helps me decide?"

2. "User tried to email a spreadsheet to someone@partner-company.com. Rule triggered on the recipient domain (external). But we have a partnership with this company and users regularly share spreadsheets with them. Is this legitimate business sharing or policy violation?"

3. "Analyst downloaded a DSPM report showing data exposures in our organization. Report references database names and record counts (no actual PII). DLP flagged it as sensitive metadata leakage. Is this a legitimate compliance report or unintended exposure?"

**Expected Output:** Evidence-based determination (True Incident or False Positive) with confidence level. If true incident: risk assessment, recommended actions, escalation path. If false positive: explanation of why rule triggered, recommendation for policy tuning.

**Limitations:** Determination requires understanding legitimate business processes (partnerships, roles, workflows). SC cannot access file contents (only filename and metadata). Cannot determine user intent. May require escalation for final decision if business context is ambiguous.

**Human Validation Points:**
1. Before marking as false positive, confirm with data owner and user's manager that sharing is legitimate
2. If suspicious, validate recipient's relationship and purpose of sharing
3. Document decision rationale for incident tracking and policy tuning feedback

**Customer Value:** DLP false positives are frustrating but investigating every alert is time-consuming. SC accelerates investigation by gathering contextual data, surfacing legitimate business reasons, and recommending disposition. This enables faster alert handling and reduces analyst burnout.

---

### UC-INV-003: Privilege Abuse and Unauthorized Access Investigation

**UC ID:** UC-INV-003
**Title:** Privilege Abuse and Unauthorized Access Investigation
**Objective:** Investigate potential privilege abuse or unauthorized data access by correlating IRM alerts, audit logs, and DSPM findings to establish whether a user accessed data outside their role and whether access was authorized.
**Target Persona:** Investigation Analyst, Security Engineer, IAM Team

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get User Risk Summary (user's access patterns and anomalies)
- Zoom Into Purview User Risk (detailed data access timeline and anomalies)
- Zoom Into Purview Data Risk (what sensitive data was accessed)
- Get Data Risk Summary (access control and authorization status)

**Key Prompts:**

1. "IRM alert: User (database analyst) accessed customer contact database they don't normally access. Timeline: accessed 50 records in 2 hours; downloaded all to CSV. Is this authorized (e.g., for new project) or privilege abuse?"

2. "Audit logs show this user's credentials were used to access a file share outside their department. User claims they didn't access it. Either credentials were compromised or user is lying. Which is more likely? What forensic evidence helps determine?"

3. "Sales user accessed the HR Benefits database (no business reason). Accessed 200 employee records; filtered by department. Is this curiosity, data theft preparation, or legitimate IT work? What's the evidence?"

**Expected Output:** Assessment of whether access was authorized or unauthorized. If unauthorized: user's access level, what data they accessed, duration of access, whether data was copied/modified. Evidence of intent. Recommendation (revoke access, investigate compromise, escalate to HR, monitor).

**Limitations:** Determining "authorization" requires knowing organizational access policies and user's assigned roles (may be missing or out-of-date). Cannot determine access intent from data alone. Cannot access file contents (only access metadata). User's credentials being used doesn't prove user did the access.

**Human Validation Points:**
1. Validate user's role and assigned access with their manager
2. If access appears unauthorized, interview user to confirm or rule out account compromise
3. If privilege abuse suspected, coordinate with HR and legal before any disciplinary action
4. If account compromise suspected, initiate credential reset and threat hunt

**Customer Value:** Privilege abuse and unauthorized access are critical insider threat and compliance concerns. SC accelerates investigation by establishing what data was accessed, by whom, when, and with what permissions. This enables faster determination of whether incident is genuine abuse, account compromise, or misunderstanding.

---

## MIP (Microsoft Information Protection & Sensitivity Labels)

### UC-MIP-001: Sensitivity Label Coverage and Usage Analysis

**UC ID:** UC-MIP-001
**Title:** Sensitivity Label Coverage and Usage Analysis
**Objective:** Assess how well sensitivity labels are deployed and used across the organization, identify coverage gaps, and measure progress toward labeling goals.
**Target Persona:** Data Steward, Compliance Officer, Chief Data Officer

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (labeled vs. unlabeled sensitive data)
- Zoom Into Purview Data Risk (sensitive data by label type and location)

**Key Prompts:**

1. "Report on our sensitivity label maturity. How many sensitive data files are labeled vs. unlabeled? Which labels are most/least used? Are labels applied correctly? What's our coverage target and timeline?"

2. "Compare label coverage by location: SharePoint (70% labeled), Exchange (30% labeled), Teams (15% labeled). Why the variation? What barriers prevent labeling in email and Teams? How can we improve?"

3. "Our data classification defines PII as Confidential, but only 25% of PII files have the Confidential label applied. What's driving the gap? Is it tooling, training, workflow, or organizational resistance?"

**Expected Output:** Maturity assessment of label deployment across the organization. Coverage metrics by location, department, and label type. Gap analysis identifying barriers to adoption. Prioritized improvement roadmap.

**Limitations:** Label usage patterns may reflect tool limitations rather than user resistance. Cannot directly assess if labels are being applied correctly (requires manual sampling). Cannot correlate labels with actual data sensitivity without detailed organizational knowledge.

**Human Validation Points:**
1. Validate label definitions with data stewards (ensure labels align with organizational sensitivity)
2. Prioritize improvement efforts based on impact on DLP/IRM/data governance initiatives
3. Coordinate with IT to deploy auto-labeling, train users, and remove friction

**Customer Value:** Sensitivity labels are foundational to modern data protection but often have low adoption and coverage. SC provides transparency into label deployment, gaps, and barriers. This enables data leaders to prioritize improvements, measure progress, and demonstrate value of labeling program.

---

### UC-MIP-002: Label Misconfiguration and Policy Misalignment Detection

**UC ID:** UC-MIP-002
**Title:** Label Misconfiguration and Policy Misalignment Detection
**Objective:** Identify sensitivity label misconfigurations (wrong protection settings, inconsistent policies across labels) and misalignment with DLP policies, and recommend remediation.
**Target Persona:** Data Security Admin, Compliance Officer

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Zoom Into Purview Data Risk (label configuration and effectiveness)
- Get Data Risk Summary (sensitive data protection by label)

**Key Prompts:**

1. "Our 'Confidential' sensitivity label has 10 protection levels configured (encryption, access control, co-authoring restrictions). Are these settings consistent with the intent of the Confidential label? Are they conflicting? What's the recommended configuration?"

2. "DLP policy 'Financial Data' is configured to block email. But Finance department files labeled 'Confidential - Finance' can still be emailed via Exchange. Why the gap? Should the label's protection policy include blocking email, or should DLP be the single point of control?"

3. "Our labels use encryption but DLP uses blocking. Is this the right approach? For a customer contact list, should we encrypt on labeling or block sharing entirely via DLP? What's the trade-off?"

**Expected Output:** Assessment of label configuration consistency and correctness. Identification of conflicts or redundancies between label protections and DLP policies. Recommendations for correcting misconfiguration.

**Limitations:** Configuration recommendations depend on organizational risk tolerance and compliance requirements. SC cannot test label behavior directly. Cannot determine if misconfiguration is accidental or intentional.

**Human Validation Points:**
1. Before changing label configurations, validate with data stewards and compliance team
2. Test configuration changes in pilot group before organization-wide rollout
3. Communicate changes to users; update training as needed

**Customer Value:** Sensitivity labels and DLP policies are often configured independently, leading to gaps, conflicts, and wasted protection. SC provides analysis to identify issues and recommend improvements. By aligning label configurations with DLP policies, organizations achieve more effective, efficient data protection with less user friction.

---

## Reporting and Analytics

### UC-REP-001: Executive Summary of Security Posture and Incidents

**UC ID:** UC-REP-001
**Title:** Executive Summary of Security Posture and Incidents
**Objective:** Generate executive-level summary of data security posture, key incidents, trends, and recommended actions for leadership review and decision-making.
**Target Persona:** CISO, VP Security, Board of Directors

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (organizational data risk overview)
- Get User Risk Summary (insider threat risk overview)
- Triage Purview Alerts (top incidents and trends)
- Zoom Into Purview Data Risk (key data exposure findings)

**Key Prompts:**

1. "Generate an executive summary of our data security posture for the board. Focus on: top 5 data risks, insider threat trends, DLP effectiveness, DSPM maturity, compliance status. Include one-page summary with recommended actions."

2. "What are our biggest data security improvements over the past quarter? What challenges remain? What's our risk trajectory (improving or degrading)? What investment would have the most impact?"

3. "Quarterly incident summary: How many breaches did we prevent with DLP? How many insider threats did we detect and remediate? What patterns indicate systemic issues vs. isolated incidents? What's our recommendation to leadership?"

**Expected Output:** One-page executive summary highlighting organizational data security posture, key metrics, trends, and prioritized recommendations. Should use business language, not technical jargon. Should quantify impact of security investments.

**Limitations:** Executive summary depends on complete, accurate data from DLP, IRM, DSPM, audit logs. If any system has data quality issues, summary will be affected. Cannot attribute prevented breaches directly to SC. Trends require historical data (at least 3 months).

**Human Validation Points:**
1. Have data security leadership review summary before presenting to board
2. Validate trends and recommendations with incident investigations and process changes
3. Acknowledge data gaps or limitations in metrics

**Customer Value:** CISOs struggle to communicate data security value and risk to business leadership. SC enables rapid generation of executive-level summaries that quantify posture, highlight improvements, and recommend priorities. This supports better decision-making by leaders and justifies continued investment in security.

---

### UC-REP-002: Post-Incident Report and Breach Summary

**UC ID:** UC-REP-002
**Title:** Post-Incident Report and Breach Summary
**Objective:** Generate comprehensive post-incident report documenting breach timeline, scope, evidence, impact, and remediation actions, suitable for regulatory notification and internal documentation.
**Target Persona:** Incident Commander, Legal, Compliance Officer

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Zoom Into Purview Data Risk (what data was breached)
- Zoom Into Purview User Risk (user involvement, timeline)
- Get User Risk Summary (user risk profile)
- Get Data Risk Summary (data sensitivity and impact)

**Key Prompts:**

1. "Generate a post-incident report for a confirmed data breach. Include: incident summary, scope (what data, how many records, which users), timeline, root cause, detection and response actions, impact assessment, and remediation steps. Use professional language suitable for regulatory notification."

2. "Our incident investigation found that a contractor's account was compromised and used to access customer data for 48 hours before detection. Generate a summary suitable for notifying the state AG and affected customers. Include timeline, scope, protective steps we've taken, and customer notification letter template."

3. "Compile incident forensics into a report. Evidence: DLP alert (suspicious email), IRM alert (account anomaly), audit logs (data access), timeline reconstruction. Summarize for CISO and legal team. What did attacker access? When? How did we detect and respond?"

**Expected Output:** Professional post-incident report including incident overview, scope quantification (records/data impacted), timeline, forensic evidence and root cause, response actions taken, damage assessment, and recommended preventive measures. Should be suitable for regulatory notifications and customer communication.

**Limitations:** Report quality depends on completeness and accuracy of investigation data. SC cannot access confidential file contents. If investigation has gaps, report will reflect those gaps. SC cannot make legal judgments about regulatory obligations.

**Human Validation Points:**
1. Have legal and compliance teams review report before regulatory notification
2. Validate timeline and forensic evidence against source logs and investigation notes
3. Redact sensitive information before sharing report with external parties
4. Coordinate with comms/PR on customer notification messaging

**Customer Value:** Post-incident reporting is complex, time-consuming, and high-stakes. SC accelerates report generation by compiling evidence, establishing timeline, and structuring findings in professional format. This enables faster regulatory notification and customer communication while ensuring comprehensive documentation.

---

### UC-REP-003: Compliance Status and Audit Readiness Assessment

**UC ID:** UC-REP-003
**Title:** Compliance Status and Audit Readiness Assessment
**Objective:** Assess organizational data security compliance status against regulatory requirements, identify gaps, and prepare for external audits by documenting controls, evidence, and remediation plans.
**Target Persona:** Compliance Officer, Audit Lead, CISO

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (data protection controls: classification, labeling, encryption)
- Zoom Into Purview Data Risk (sensitive data locations, access controls, remediation status)
- Zoom Into Purview User Risk (insider threat controls and detection)
- Get User Risk Summary (access governance and anomaly detection)

**Key Prompts:**

1. "Map our Purview controls to HIPAA Security Rule requirements. Do we have detective and preventive controls for each requirement (access controls, encryption, audit logging, incident response)? Which controls are strong? Which need improvement? Create a compliance matrix for audit."

2. "Generate a SOC 2 Type II audit report summary. For each SOC 2 security criterion, document the control we've implemented (DLP policies, IRM detection, DSPM posture management), evidence of control effectiveness, and time period under review. What evidence do auditors need?"

3. "We're preparing for a PCI-DSS audit. Map our current data protection controls to PCI-DSS requirements. Which requirements do we fully meet? Which need additional controls? Generate a gap analysis and remediation roadmap with effort estimates."

**Expected Output:** Compliance mapping document showing organizational controls aligned to regulatory requirements. Gap analysis identifying which requirements are met, partially met, or unmet. Evidence inventory. Prioritized remediation roadmap suitable for auditor review.

**Limitations:** Compliance mapping is regulatory-specific; SC must understand relevant framework to provide accurate mapping. Requirements change frequently. Determining "evidence of control effectiveness" requires careful interpretation of incident metrics.

**Human Validation Points:**
1. Have compliance team and external auditors review mapping before finalizing
2. Validate that evidence cited actually demonstrates control, not just control existence
3. Get executive sponsor for remediation roadmap; prioritize based on audit schedule and impact

**Customer Value:** Compliance audits are resource-intensive and time-consuming. SC accelerates audit preparation by mapping controls to requirements, identifying gaps, compiling evidence, and prioritizing remediation. This reduces audit stress and likelihood of significant findings.

---

## Operations

### UC-OPS-001: Security Readiness Assessment and Customer Workshop Preparation

**UC ID:** UC-OPS-001
**Title:** Security Readiness Assessment and Customer Workshop Preparation
**Objective:** Conduct rapid assessment of customer's data security readiness, identify gaps, and prepare workshop agenda and materials to guide customer toward maturity.
**Target Persona:** CSA, Consulting Engineer, Customer Success Manager

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Get Data Risk Summary (current data risk posture)
- Get User Risk Summary (insider threat maturity)
- Zoom Into Purview Data Risk (data protection gaps)
- Zoom Into Purview User Risk (insider threat detection maturity)

**Key Prompts:**

1. "Conduct a 15-minute readiness assessment of this customer's data security program. Evaluate: DLP policy coverage (gaps?), sensitivity label deployment (adoption?), insider threat detection (false positives?), DSPM maturity (exposure visibility?). Summarize maturity (Basic/Intermediate/Advanced/Mature) for each area. What are quick wins?"

2. "Customer is planning a 2-day workshop with our team. Generate workshop agenda covering: data classification workshop (define sensitive data), DLP policy design (build coverage roadmap), insider threat detection (tune IRM), DSPM posture (remediate exposures). Include pre-workshop assessment, workshop activities, and post-workshop follow-up."

3. "Customer wants to improve DLP effectiveness. They have 20 active policies with 10% false positive rate. Generate a workshop outline: policy audit (which policies are useful?), gap analysis (what coverage is missing?), false positive reduction (tune rules), roadmap (6-month improvement plan)."

**Expected Output:** Readiness assessment quantifying customer's maturity in each data security domain, identifying strengths and gaps, and recommending focus areas. Workshop agenda structured with pre-work, classroom activities, hands-on labs, and post-workshop follow-up.

**Limitations:** Assessment depends on accurate input about customer's current state. SC can identify gaps but cannot know customer's business priorities or budget constraints (CSA must add context). Workshop agenda is template; CSA must customize based on customer feedback.

**Human Validation Points:**
1. Validate assessment with customer's security leadership before finalizing workshop agenda
2. Customize workshop to match customer's specific priorities and timeline
3. Confirm workshop attendees understand SC framework and limitations before the event

**Customer Value:** CSAs regularly conduct readiness assessments and facilitate customer workshops. SC accelerates this work by rapidly generating assessment, identifying gaps, and creating workshop structure. This enables CSAs to spend less time on preparation and more time on high-value customer engagement.

---

### UC-OPS-002: Purview Tuning and Policy Optimization Playbook

**UC ID:** UC-OPS-002
**Title:** Purview Tuning and Policy Optimization Playbook
**Objective:** Create systematic playbook for continuously tuning and optimizing Purview DLP, IRM, and DSPM policies based on incident data, false positive trends, and organizational changes.
**Target Persona:** Data Security Admin, Security Operations Manager

**SC Experience:** Standalone

**SC System Capabilities Used:**
- Triage Purview Alerts (alert trends and patterns)
- Zoom Into Purview Data Risk (DLP policy effectiveness)
- Zoom Into Purview User Risk (IRM detection tuning)

**Key Prompts:**

1. "Create a monthly Purview tuning playbook for the security team. Steps: (1) analyze DLP false positive rate for each policy, (2) identify policies with low effectiveness (few matches), (3) review IRM false positives by indicator type, (4) assess DSPM coverage, (5) prioritize tuning efforts. Include decision criteria for disabling, modifying, or creating policies."

2. "Generate a DLP policy maintenance calendar. Quarterly: review policy coverage gaps. Monthly: audit false positive rate and tune rules. Weekly: review top incidents and recommend immediate policy adjustments. For each review, what metrics matter? What actions should we take?"

3. "After a major organizational change (acquisition, spin-off, new product line), what Purview policies need updating? Generate a checklist: DLP rules for new data types, IRM baselines for new user populations, DSPM exposure inventory for new infrastructure."

**Expected Output:** Structured playbook with regular cadence (weekly, monthly, quarterly) for tuning Purview policies. For each cycle: metrics to monitor, analysis to perform, decision criteria, and actions to take. Should include templates for documenting policy changes and rollback procedures.

**Limitations:** Playbook is template; actual tuning requires domain expertise and business context. Tuning decisions often require balancing competing goals. Cannot fully automate tuning without understanding customer's risk tolerance and compliance requirements.

**Human Validation Points:**
1. Customize playbook to match customer's risk tolerance and operational maturity
2. Before implementing tuning recommendations, test on sample data and validate with stakeholders
3. Document all tuning decisions and their impact for audit trail and future reference

**Customer Value:** Policy tuning is continuous but often ad-hoc. SC provides structured approach with regular cadence, clear metrics, and documented playbook. This enables more systematic, effective policy optimization. Over time, customers achieve higher DLP effectiveness, lower false positive rates, and better insider threat detection.

---

## Summary: Use Case Coverage

| Category | Count | Coverage |
|----------|-------|----------|
| DLP (Data Loss Prevention) | 4 | Alert triage, policy investigation, false positive analysis, coverage gaps |
| IRM (Insider Risk Management) | 4 | Alert triage, user timeline analysis, cross-signal correlation, risk score interpretation |
| DSPM (Data Security Posture) | 2 | Exposure assessment, classification gap analysis |
| Investigation (Multi-Signal) | 3 | Data leakage investigation, FP determination, privilege abuse |
| MIP (Sensitivity Labels) | 2 | Label coverage analysis, misconfiguration detection |
| Reporting | 3 | Executive summary, post-incident report, compliance assessment |
| Operations | 2 | Readiness assessment, tuning playbook |
| **TOTAL** | **22** | **Comprehensive coverage of Purview data security workflows** |

---

## Key Principles Across All Use Cases

1. **Human-in-the-Loop:** All use cases assume analyst validates SC output before action.
2. **Realistic Prompts:** All prompts leverage only the 6 validated Purview plugin capabilities and 4 SC agents.
3. **Honest Limitations:** Each use case explicitly documents what SC cannot do.
4. **Validation Points:** Each use case includes mandatory human validation steps.
5. **Customer Value:** Each use case quantifies time saved or quality improved.

---

**Document Status:** Production Ready
**Last Validated:** April 2026
**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development
