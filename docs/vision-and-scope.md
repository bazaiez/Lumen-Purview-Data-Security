# Vision and Scope: Lumen — Purview Data Security

## Vision Statement

Empower data security teams to investigate, respond, and report on Purview security incidents 3x faster through structured Security Copilot workflows and prompt engineering. Transform Security Copilot from a general-purpose tool into a specialized investigator that understands Purview data models, detection patterns, and compliance requirements.

## Problem Statement

Data security teams using Microsoft Purview face three acute challenges that slow incident response and increase operational burden:

1. **Alert Fatigue and Triage Overhead**: DLP policies generate hundreds of alerts daily; analysts spend 20-30% of time triaging false positives with minimal context about data criticality, user intent, or remediation options.

2. **Investigation Complexity**: Correlating signals across DLP, IRM, DSPM, MIP, and Audit requires jumping between five+ consoles, each with different query languages, data models, and filtering capabilities. Critical context (e.g., data classification, IRM permissions, historical baselines) is scattered across systems.

3. **Reporting Burden**: Compliance teams manually gather evidence, build correlation narratives, and draft executive summaries. Creating a single post-incident report or compliance audit response takes 4-8 hours of manual work per incident.

4. **Knowledge Gaps**: Security teams often lack deep expertise in all Purview products. New analysts need 6-12 weeks to become independent. Consultants struggle to explain Purview behavior to customers without extensive prep.

Security Copilot has the potential to address all four challenges—but only if used with intentional prompt engineering, structured workflows, and clear guardrails. This framework provides the architecture to unlock that potential while being honest about current limitations.

## Scope

### What Is IN Scope

**Products and Features:**
- Data Loss Prevention (DLP)
- Information Rights Management (IRM)
- Data Security Posture Management (DSPM)
- Microsoft Information Protection (MIP) including sensitivity labels
- Audit logs and investigation surfaces
- Risk and compliance reporting

**Capabilities:**
- Investigation workflow acceleration (triage, correlation, contextualization)
- Prompt engineering for Purview scenarios
- Analyst role acceleration (SOC, compliance, admin, leadership)
- Customer enablement and workshop support
- Consulting and demo workflows
- Executive reporting and trend analysis
- Knowledge-base development for Purview behavior and capabilities

**Outputs:**
- Playbooks for common investigation scenarios
- Reusable prompt templates and taxonomies
- Investigation decision trees
- Reporting templates and automation
- Knowledge articles and reference docs
- Best practices for CSA and customer teams

### What Is OUT of Scope

- Endpoint security, device compliance, or endpoint DLP enforcement
- Identity-focused investigations (identity compromise response, etc.)
- Network security, firewall management, or traffic analysis
- Security Copilot plugin development or custom connector creation
- Production automation or write-back actions from Security Copilot
- Real-time, at-scale policy enforcement or incident response automation
- Purview integration with third-party SIEM or SOAR platforms
- Cloud app security or cloud-native workload protection
- Specialized forensics requiring deep knowledge of Purview internals or undocumented behaviors

## Where Security Copilot Adds Most Value

**Immediate High-Impact Use Cases:**

1. **Investigation Summarization**: Copilot rapidly synthesizes findings from multiple alerts, audit logs, and data contexts into a coherent narrative, reducing investigation time from hours to minutes.

2. **Alert Triage Acceleration**: Copilot interprets DLP alert patterns, user profiles, and historical context to flag false positives and prioritize genuine risk—cutting triage time by 50%.

3. **Cross-Signal Correlation**: When an analyst has relevant data from multiple Purview surfaces (DLP alert + IRM permissions + DSPM findings + user audit trail), Copilot identifies connections and threat patterns that manual review would miss.

4. **Executive Reporting**: Copilot translates technical incident details into board-ready summaries, trend analysis, and risk heat maps without requiring manual PowerPoint work.

5. **Explaining Technical Findings to Non-Experts**: Copilot reframes policy violations, technical controls, and compliance findings in business language suitable for leaders and non-technical stakeholders.

6. **Proactive Hunting Assistance**: Copilot helps analysts construct hypotheses and refinement strategies for hunting investigations (e.g., "find patterns of external sharing of sensitive customer data").

## Where Security Copilot Does NOT Add Value (Yet)

**Current Limitations (Be Honest With Customers):**

- **Real-Time Blocking**: Security Copilot is investigative, not preventive. It cannot make real-time policy decisions or block actions. Purview enforcement controls remain the enforcer.
- **Policy Configuration or Enforcement**: Copilot cannot write DLP policies, create IRM templates, or push configuration changes. Policy design remains human-driven.
- **Write-Back Actions**: Copilot cannot automatically remediate incidents (e.g., remove sharing, revoke IRM rights, quarantine files). Remediation must be human-authorized.
- **Highly Specialized Forensics**: Deep forensic analysis of Purview internals, performance tuning, or debugging unusual Purview behaviors requires Microsoft support or expert consulting.
- **Scenarios Requiring Data Beyond Copilot's Access**: If investigation requires data not available in Purview (e.g., file content scan, deep network telemetry, endpoint logs), Copilot cannot provide answers.
- **Predictive Risk Scoring**: Copilot cannot reliably forecast which users are likely to cause incidents or which data is likely to be breached (though it can analyze historical patterns).

## Success Criteria

### Team Success Metrics

1. **Time Savings**: Analysts close DLP investigations 40-50% faster; compliance teams reduce report-generation time by 4+ hours per incident.
2. **Quality Improvement**: Investigation narratives are more complete and data-driven; fewer missed correlations.
3. **Adoption and Confidence**: 80%+ of target user roles actively use Copilot prompts; self-reported confidence in investigations increases.
4. **Knowledge Distribution**: New analysts reach independent productivity 25% faster; expert knowledge becomes more codified and teachable.

### Customer Success Metrics

1. **Demo Effectiveness**: CSAs report ability to run interactive, compelling Purview demos without 4-8 hours of prep per customer.
2. **Customer Enablement**: Customers adopt Security Copilot in their own SOC workflows and report measurable improvements in alert triage or compliance reporting.
3. **Consulting Velocity**: Consulting engagements complete faster, reduce billable hours, and improve customer satisfaction.
4. **Adoption**: 40%+ of customers with Purview in scope adopt recommended Copilot workflows within 6 months of engagement.

### Repository Success Metrics

1. **Content Quality**: All prompts are tested and documented; false-positive rate of Copilot recommendations is well understood and flagged.
2. **Versioning Discipline**: Version history is clear; prompt changes are tracked and justified.
3. **Community Contribution**: At least 2-3 CSA teams contribute prompts, playbooks, or knowledge articles per quarter.
4. **Measurement**: Repository includes clear guidance on how to measure ROI of Copilot adoption in customer environments.

