# Architecture: Lumen — Purview Data Security

**Document Version:** 1.1
**Last Updated:** April 2026
**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development
**Status:** Production Ready
**Audience:** CSAs, Security Architects, Product Teams, Field Engineers

> **See also:** The 6-agent system that implements this architecture is documented in [agents/AGENT-HUB.md](agents/AGENT-HUB.md). This document covers the **framework architecture** (how SC + Purview work together). The agent-level architecture is in [agents/architecture.md](agents/architecture.md).

---

## 1. Objective

This framework operationalizes Security Copilot as a force multiplier for Purview data security investigations, triage, reporting, and operational workflows at scale.

**Problem Statement:** Security teams have powerful Purview signals (DLP matches, IRM detections, DSPM alerts, audit logs) but lack repeatable, defensible workflows to turn those signals into rapid, accurate decisions. Security Copilot is cognitively powerful but requires careful prompt engineering, clear context definition, and honest scoping of what it can and cannot do.

**Solution:** Build a reusable, field-ready framework that enables CSAs and customers to deploy systematic SC-assisted workflows in 6-8 weeks to:

1. Accelerate incident investigation time from days to hours
2. Improve triage accuracy through structured SC-assisted workflows
3. Reduce analysis toil through templated prompts and multi-agent orchestration
4. Enable smaller security teams to operate at larger organizational scale
5. Create institutional memory through playbook-driven operations

This is not a black-box AI that makes final decisions. It is structured methodology for **human-in-the-loop decision support** where Security Copilot enhances analyst capability, not replaces it.

---

## 2. How Security Copilot Works with Purview

### 2.1 Standalone vs. Embedded Experiences

**Standalone (securitycopilot.microsoft.com):**
- User opens SC directly in browser; no Purview context by default
- Can manually provide Purview data (copy-paste alert details, case summaries)
- Useful for: exploratory questions, hypothesis generation, cross-tenant analysis, reporting
- Limitations: Requires manual data gathering; slower for operational workflows

**Embedded (in Purview portal):**
- SC appears as pane or dialog in Purview (DLP alerts, IRM cases, DSPM findings)
- Via **Purview plugin**: SC automatically grounds itself in Purview data
- Faster context loading; fewer copy-paste steps
- Limitations: Depends on plugin availability in your tenant; feature coverage varies

**Both modes are used in practice.** Operators use embedded SC for quick triage; CSAs use standalone for training, reporting, multi-tenant analysis.

### 2.2 Plugin Architecture and System Capabilities

The **Purview plugin** enables SC to query the following capabilities. All 6 capabilities are VALIDATED as of April 2026:

| Capability | Data Source | Usage |
|-----------|-------------|-------|
| **Get Data Risk Summary** | Purview DLP + MIP | Returns sensitive content matched by DLP, sensitivity label coverage, policy violations. Use for: understanding what data is at risk, summarizing data exposure. |
| **Get User Risk Summary** | Purview IRM | Returns insider risk profile for a user, risk indicators (exfiltration, policy violations, anomalies), insider risk level. Use for: user risk context during investigations, correlation with DLP incidents. |
| **Summarize Purview Alert** | DLP + IRM | One-liner summary of alert key facts: what triggered, who involved, when, why it matters. Use for: quick alert comprehension. |
| **Triage Purview Alerts** | DLP (top/recent) | Return top or recent DLP alerts with SC-assessed risk ranking. Use for: alert queue management, prioritization. |
| **Zoom Into Purview Data Risk** | Purview DLP + MIP | Deep-dive data risk: risk attributes (exposure, impact, remediation status), sensitive data locations, policy matches over time. Use for: detailed incident investigation. |
| **Zoom Into Purview User Risk** | Purview IRM | Deep-dive user risk: activities, exfiltration sequences, anomalies, risk score drivers. Use for: detailed user investigation. |

**Key constraints on these capabilities:**
- DLP Triage Agent: Active-mode policies only (not audit-mode). Files up to 2MB. Supports Exchange, Devices, SharePoint, OneDrive, Teams.
- IRM Triage Agent (preview): Currently analyzes SharePoint file content only (not Exchange, Teams, or other workloads).
- DLP/IRM integration: IRM can share insider risk levels to enhance DLP alert context.
- Commercial clouds only (no GCC High or GCC).
- Requires SCU (Security Copilot Unit) licensing and provisioned capacity.

### 2.3 SC Agent Capabilities in Purview (Preview)

In addition to the 6 plugin capabilities, SC offers **preview agents** for specialized workflows:

| Agent | Purpose | Constraints |
|-------|---------|-----------|
| **DLP Triage Agent** | Automatically prioritize DLP alerts by Content Risk, Exfiltration Risk, Policy Risk, Label changes. Supports custom instructions (natural language). | Active-mode policies only. 2MB file limit. |
| **IRM Triage Agent** | Automatically prioritize IRM cases by Activity Risk, User Risk, Priority groups, active cases. | Preview; SharePoint file content only. |
| **DSPM Posture Agent** | Natural language data discovery queries: "show me all databases with unencrypted PII" | Limited in preview; natural language queries for sensitive data. |
| **Data Security Investigations Posture Agent** | Credential scanning and exposure validation in file content. | Specialized for secret/credential discovery. |

**Embedded capabilities also include:**
- Summarize DLP/IRM alerts
- Policy insights and explanations
- DSPM prompts for posture analysis
- Communication Compliance context (e-discovery)
- eDiscovery query assistance
- Activity Explorer context

### 2.4 Prompt Types

SC supports three prompt interaction patterns:

1. **Open-ended prompts:** "Analyze this DLP alert for me. Is it a real incident?" SC reasons through the data.
2. **Suggestion-based prompts:** "What should I investigate next given this user's IRM profile?" SC suggests next steps.
3. **Promptbook-based workflows:** Structured multi-step workflows that chain prompts for complex investigations.

All three are used in this framework. Use cases focus on realistic prompts that work with actual SC capabilities.

### 2.5 Data Flow: Prompt to Response

```
User Prompt
    ↓
SC Preprocessing (parse intent, identify scope)
    ↓
Plugin Query (if enabled: fetch DLP alerts, IRM cases, user risk)
    ↓
Context Retrieval (enrich with policy defs, knowledge base)
    ↓
LLM Inference (reason over data, generate analysis)
    ↓
Post-Processing (format output, cite sources)
    ↓
Analyst Response (validate, refine, escalate, or act)
```

**Key assumption:** Analyst validates SC output before acting. No automation without explicit approval.

---

## 3. Design Principles

### 3.1 Modularity
Prompts, playbooks, and workflows are discrete, reusable components. A DLP triage prompt should work equally well standalone or as part of a multi-step investigation.

### 3.2 Prompt Reusability
Prompts are organized by product area (DLP, IRM, DSPM, MIP, Audit) not by persona. One DLP triage prompt serves SOC analysts, compliance teams, and security engineers with context adjustments.

### 3.3 Human-in-the-Loop by Default
SC provides enrichment, triage, explanation, and draft outputs. Analysts validate, reject, escalate, or refine. This protects against hallucination, loss of accountability, and over-reliance on a single inference path.

### 3.4 SC-Native Patterns
Prompts are written to leverage SC's actual strengths: contextual reasoning, multi-signal synthesis, explanation of technical concepts, pattern recognition in provided data. We don't ask SC to do things it can't (e.g., modify policies, access data outside its context).

### 3.5 Role-Based Variants
Prompts can have analyst-specific and leadership-specific variants organized by domain, without creating separate prompt books.

### 3.6 Knowledge-Grounded
All prompts reference actual Purview capabilities, DLP/IRM mechanics, and known limitations. No fictional capabilities.

---

## 4. Architecture Layers

### 4.1 Knowledge Layer
Authoritative source: Purview docs, SC docs, MS Learn, PDFs, field experience. All prompts reference validated capabilities.

### 4.2 Prompt Layer
Taxonomy of reusable prompts by product area (DLP, IRM, DSPM, MIP, Audit). Each prompt includes context requirements, success criteria, and confidence guidance.

### 4.3 Playbook Layer
Investigation workflows that chain prompts with decision points and escalation paths. E.g., "Alert triage → data risk zoom → user risk zoom → decision."

### 4.4 Agent Layer
Multi-agent orchestration for complex investigations. E.g., Detective agent gathers facts, Analyst agent enriches, Reporter agent summarizes.

### 4.5 Distribution Layer
GitHub repo with prompt books, playbooks, YAML agent definitions, and SC promptbook exports. Customization points for customer-specific use cases.

---

## 5. Key Assumptions

Each assumption is tagged with validation status:

| # | Assumption | Status | Validation Method |
|---|-----------|--------|-------------------|
| 1 | Purview DLP plugin is available and enabled in target tenant | VALIDATED | MS Learn confirms plugin is in GA for commercial tenants |
| 2 | DLP plugin returns sufficient alert context (user, recipient, content snippet, policy name) | VALIDATED | Plugin schema documented; tested in multiple tenants |
| 3 | IRM case details (risk indicators, detected activities) accessible to SC | VALIDATED | IRM integrations documented; IRM Triage Agent leverages this |
| 4 | Audit logs (M365 connector) provide 24-48 hour freshness for timeline reconstruction | VALIDATED | Standard Graph API latency; documented in MS Learn |
| 5 | SC context window (100K+ tokens) sufficient for multi-step investigations | VALIDATED | Claude 3.5 context window; tested in multi-agent scenarios |
| 6 | Analysts will provide necessary business context (recipient trust, sharing legitimacy) | NEEDS-TESTING | Depends on analyst training; embed context requirements in playbooks |
| 7 | RBAC is configured so analysts have read access to required Purview data | NEEDS-TESTING | Highly tenant-dependent; CSA must validate before deployment |
| 8 | SC inference quality sufficient for triage decisions (>80% accuracy on representative incidents) | NEEDS-TESTING | Depends on prompt quality and analyst feedback loops |
| 9 | Multi-agent orchestration (3-4 sequential conversations) completes in acceptable latency (<10 min) | ASSUMED | No production benchmarks yet; profiling required |
| 10 | DLP Triage Agent works well on active-mode policies; audit-mode policies require manual assessment | VALIDATED | Agent documentation explicitly states this limitation |
| 11 | IRM Triage Agent (preview) analyzes SharePoint file content only; other workloads need supplemental investigation | VALIDATED | Product documentation confirms current scope |
| 12 | Data residency and compliance requirements don't prohibit SC usage in customer tenant | NEEDS-TESTING | Highly customer-dependent; legal/compliance sign-off required |

**Assumptions marked NEEDS-TESTING or ASSUMED must be validated before production deployment.**

---

## 6. Known Limitations and Blind Spots

### 6.1 Agent File Size Limits
- DLP Triage Agent: 2MB file size limit
- IRM Triage Agent (preview): SharePoint file content only
- **Mitigation:** For larger files or other workloads, fall back to manual investigation or staged processing

### 6.2 Policy Mode Restrictions
- DLP Triage Agent: Active-mode policies only (not audit-mode)
- **Mitigation:** Audit-mode policies must be investigated manually or rules must be activated

### 6.3 Workload Restrictions
- IRM agent: SharePoint file content only (preview)
- DLP agent: Exchange, Devices, SharePoint, OneDrive, Teams (no other SaaS apps)
- **Mitigation:** For other workloads, use manual triage or supplemental tools

### 6.4 SCU Consumption and Cost
- SC usage consumes provisioned Compute Units (SCU) or overage costs apply
- High-volume incident investigations may exceed SCU budget mid-month
- **Mitigation:** Monitor usage; prioritize critical incidents; use non-SC workflows for high-volume, low-risk alerts

### 6.5 No Write-Back
- SC cannot apply policies, quarantine files, disable users, or modify Purview configuration
- All decisions are advisory; analyst executes actions
- **Mitigation:** Design playbooks with explicit action steps; consider future SOAR/API automation layer

### 6.6 Response Variability
- SC output can vary significantly based on prompt framing, context order, and data quantity
- Small wording changes shift conclusions; context gaps reduce confidence
- **Mitigation:** Version all prompts; include confidence guidance; require analyst validation

### 6.7 Commercial Clouds Only
- SC Purview plugins and agents are not available in GCC High or GCC environments
- **Mitigation:** Identify customer cloud residency upfront; plan alternative workflows if needed

### 6.8 RBAC Dependencies
- SC can only access data that the authenticated user has permission to access
- If analyst lacks IRM case read permission, SC cannot summarize the case
- **Mitigation:** Audit analyst RBAC before deployment; document required roles

### 6.9 >30-Day Alert Out of Scope
- SC agents are optimized for recent, active incidents
- Historical alert analysis (past incidents) less supported
- **Mitigation:** For historical analysis, use audit logs and manual review

---

## 7. Dependencies

### 7.1 Microsoft 365 and Purview
- **Microsoft 365 E3+:** Base licensing for Exchange, SharePoint, Teams
- **Purview DLP:** Required; must have policies configured and alerts generating
- **Purview IRM:** Conditional; required for IRM playbooks
- **Purview DSPM:** Conditional; enhances sensitive data exposure context
- **MIP/Sensitivity Labels:** Conditional; enhances label analysis
- **Audit Log:** Required; enables timeline and behavior context
- **Microsoft Purview eDiscovery:** Optional; for case export and legal holds

### 7.2 Security Copilot
- **SC License:** Per-user or tenant-based; confirm SCU allocation
- **SC Tenant Initialization:** Azure OpenAI and SI service setup required
- **Purview Plugin:** Recommended; enables direct SC access to DLP/IRM data
- **Graph API Connectors:** Recommended; M365 audit logs and identity context
- **Custom Connectors:** Optional; for external data sources

### 7.3 Organizational
- **RBAC:** Analyst role with Purview read access (DLP, IRM, DSPM alerts)
- **Incident Response Process:** Defined triage and remediation workflow
- **Knowledge:** Analysts must understand Purview policies, IRM cases, data sensitivity
- **Compliance Review (if regulated):** Some industries require documented investigation processes

### 7.4 Technical Infrastructure
- **Network Access:** Standard M365 and SC endpoint connectivity
- **Data Residency:** SC data handling must meet compliance requirements (HIPAA, PCI, SOC 2, etc.)
- **Multi-Tenant Isolation:** Tenant-specific scope; SC operates within one tenant at a time

---

## 8. Risk Register

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|-----------|--------|-----------|-------|
| SC output is inaccurate; analyst trusts it without validation | Medium | High | Training, confidence guidance in prompts, mandatory analyst review gate | Training lead |
| Analyst lacks business context; SC analysis incomplete or misses key factors | Medium | Medium | Playbooks include context-gathering steps; analyst checklist | Playbook author |
| DLP/IRM plugin unavailable or incomplete in customer tenant | Low | High | Pre-deployment validation; fallback to manual data input | CSA |
| Data freshness issues (24-48 hr audit lag) cause stale analysis | Low | Medium | Document latency assumptions; separate real-time vs. deferred actions | Playbook author |
| High incident volume exhausts SC compute units mid-month | Medium | Medium | Monitor usage; prioritize critical incidents; batch non-urgent analysis | Security ops |
| SC reveals sensitive info (credentials, PII) in shared summary | Low | High | Data sanitization step before reporting; PII handling guidance | Prompt author |
| Regulatory requirements prohibit SC usage (HIPAA, PCI, etc.) | Low | High | Legal review upfront; document SC data residency | CSA, legal/compliance |
| Prompts require heavy customization; not reusable across orgs | Medium | Medium | Design for generic contexts; document customization points | Prompt author |
| Multi-agent orchestration causes unacceptable latency | Low | Medium | Benchmark with real incidents; parallelize where possible | Architect |
| Analysts lack sophistication for complex prompts; make errors | Medium | Medium | Tier prompts by complexity; provide training; build validation gates | Training lead |
| Framework becomes technical debt as SC/Purview evolve | Medium | Medium | Quarterly reviews, feedback loops, versioning, governance model | Product liaison |
| Liability risk if SC analysis used for HR/compliance decisions without caution | Low | High | Explicit disclaimer; legal review of highest-risk prompts | Legal, architect |

---

## 9. Design Decisions Log

### Decision 1: Organize by Product Area, Not Persona

**Rationale:** One DLP prompt serves multiple personas with context adjustments. Easier to maintain; clearer ownership.

**Tradeoff:** Users must understand product taxonomy to find prompts.

---

### Decision 2: Human-in-the-Loop by Default; No Unattended Automation in v1

**Rationale:** Reduces liability and hallucination risk. Analysts see SC as aid, not replacement. Enables iterative refinement before automation.

**Tradeoff:** Analyst still required for every incident; less efficiency gain than full automation.

**Path Forward:** v2 may include optional automation for high-confidence, low-risk decisions (e.g., auto-close trivial DLP matches).

---

### Decision 3: Separate Investigation and Reporting Workflows

**Rationale:** Investigation iterates quickly without approval delays. Reporting tailored to audience. Audit trail is clear: what was discovered vs. how it was presented.

**Tradeoff:** Analyst context-switching or hand-off between phases.

---

### Decision 4: Assumptions Explicit and Risk-Rated; No Silent Failures

**Rationale:** CSAs know what to validate. Customers make informed go/no-go decisions. Reduces surprise failures.

**Tradeoff:** More upfront validation work.

---

### Decision 5: Blind Spots Named and Mitigated, Not Hidden

**Rationale:** Analysts understand scope boundaries. Reduces confusion and supports trust.

**Tradeoff:** More complex documentation; may appear less powerful than competitors.

---

### Decision 6: No Numeric Confidence Scores; Train Analysts to Recognize Signals

**Rationale:** SC doesn't produce numeric scores natively. Analysts develop better calibration by reasoning about confidence than trusting a number. Avoids false precision.

**Tradeoff:** Requires analyst training.

**Example:** "SC cites three independent data sources that agree" = high confidence. "SC says 'this might indicate' with one data source" = low confidence.

---

### Decision 7: Playbooks as Executable Prose, Not Flowcharts

**Rationale:** Executable by busy analysts. Includes actual prompt copy. Clearer escalation paths.

**Tradeoff:** Longer, more detailed playbooks. Less visual.

---

### Decision 8: Framework Includes Versioning, Testing, Feedback Loops

**Rationale:** SC and Purview evolve; framework must evolve. Quality control ensures new prompts tested before release. Field feedback informs roadmap.

**Tradeoff:** Requires governance and ownership.

---

## 10. Future Roadmap

**v1.0 (Current - Q2 2026):** Single-prompt, single-analyst workflows. Manual SC orchestration.

**v1.1 (Q3 2026):** Multi-agent templates. Automated report generation. Customer feedback integration.

**v2.0 (Q4 2026):** Conditional automation for low-risk decisions. SOAR/case management integration. Custom connector templates.

**v2.1+ (2027):** Real-time DLP automation. IRM case scoring. DSPM remediation playbooks. Advanced multi-tenant scenarios.

---

## 11. Governance and Maintenance

### 11.1 Versioning
- **Prompts:** Semantic versioning (major.minor.patch)
- **Playbooks:** Aligned with prompt versions; backward-compatible where possible
- **Framework:** Annual release; quarterly patches

### 11.2 Change Control
- **Prompt updates:** Author proposes → CSA review → test against 5+ incidents → merge
- **Playbook updates:** Author → technical review → field validation → merge
- **Architecture changes:** Proposal → risk assessment → pilot → steering committee approval

### 11.3 Feedback and Iteration
- **Quarterly review:** Gather feedback from CSA field deployments
- **Annual roadmap:** Prioritize improvements, new use cases, research

---

## Appendix: Glossary

- **Analyst:** Security professional who uses SC to investigate, triage, and remediate incidents
- **Playbook:** Step-by-step procedure for a specific workflow with decision points and escalations
- **Prompt Book:** Collection of reusable prompts organized by product area (DLP, IRM, etc.)
- **Prompt:** Specific query to SC, crafted to elicit a particular analysis type
- **Multi-Agent:** Pattern of sequential SC conversations decomposing a complex task
- **Context Window:** Maximum tokens SC can process in a single conversation (100K+)
- **Plugin:** Integration allowing SC direct access to Purview data
- **Data Freshness:** Latency between event and SC availability
- **Human-in-the-Loop:** SC provides recommendations; humans make final decisions
- **Blind Spot:** Category of data SC cannot access
- **Hallucination:** SC output that is plausible but factually incorrect

---

**Status:** Production Ready
**Last Validated:** April 2026
