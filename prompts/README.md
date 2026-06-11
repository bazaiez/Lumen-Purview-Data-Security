# 📝 Lumen — Prompt Books

Complete prompt library for Security Copilot + Purview Data Security integration. 

**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development

> 💡 **Generate new prompts from scratch:** Use the [⚡ Prompt Generator Agent](../agents/prompt-generator-agent.md) to create prompts for any use case not covered here.

---

## 📚 Prompt Books Overview

### 🔎 1. Audit and Investigation (`audit/prompt-book.md`)
**Focus:** Audit log analysis, timeline reconstruction, cross-signal correlation

10 prompts covering:
- Incident timeline reconstruction
- Data exfiltration pattern identification
- User risk signal correlation
- Policy violation root cause analysis
- Compliance audit trail validation
- Anomalous sign-in detection
- Data flow mapping
- User access review automation
- Training gap analysis
- E-discovery and regulatory response

**Best for:** Security operations, incident response, forensic investigations

**Validation Status:** 8 validated patterns (core audit capabilities), 2 aspirational (cross-plugin correlation)

---

### 🏷️ 2. Information Protection and Sensitivity Labels (`mip/prompt-book.md`)
**Focus:** Label coverage, misconfiguration detection, auto-labeling effectiveness

10 prompts covering:
- Label adoption and coverage analysis
- Mislabeled content detection
- Auto-labeling performance assessment
- Regulatory label validation (GDPR/HIPAA/PCI/SOX)
- Label taxonomy optimization
- Labeled data access analytics
- Policy enforcement and governance
- Label protection settings review
- Email and attachment labeling compliance
- Label program KPI metrics

**Best for:** Data governance, compliance teams, label strategy

**Validation Status:** 6 validated (coverage, compliance, protection review), 4 aspirational (cross-system visibility)

---

### 📊 3. Executive Reporting (`executive/prompt-book.md`)
**Focus:** Leadership summaries, board reports, incident briefs

8 prompts covering:
- Monthly data security risk summary
- Incident impact briefs for leadership
- Compliance audit readiness
- Breach notification and disclosure
- Quarterly security metrics for board
- Technical briefing for CISO/CTO
- Customer-facing security posture
- Risk-based remediation roadmap

**Best for:** CISO/CTO, board reporting, incident communication, executive briefs

**Validation Status:** 7 validated (SC's core synthesis capability), 1 aspirational (cross-org risk aggregation)

---

### 🎓 4. Customer Workshop and Demo (`workshop/prompt-book.md`)
**Focus:** CSA-ready demos, workshop scenarios, talking point generators

10 prompts covering:
- Live demo: Quick data risk summary
- Live demo: High-risk user access
- Workshop scenario: Incident investigation simulation
- Workshop scenario: Label taxonomy co-design
- Custom talking points by audience
- Hands-on lab: Customer-guided exploration
- Post-incident validation demo
- KPI dashboard workshop
- Guided risk assessment training
- Sales enablement and demo scripts
- Pre-built demo scenarios library

**Best for:** Sales, CSA, customer workshops, product demos

**Validation Status:** All validated (proven customer-friendly outputs)

---

### ⚙️ 5. Operations and Configuration (`operations/prompt-book.md`)
**Focus:** Policy setup, configuration guidance, deployment readiness

10 prompts covering:
- Sensitivity label configuration planning
- DLP policy configuration and testing
- Auto-labeling rule setup and tuning
- Policy audit and configuration review
- Access control validation
- Deployment readiness assessment
- User training and adoption program
- Incident response procedure documentation
- Continuous improvement governance
- Tool integration and data architecture

**Best for:** Security operations, policy configuration, deployment planning

**Validation Status:** 8 validated (operational guidance), 2 aspirational (integration with external systems)

---

### 🔬 6. DFIR Deep Investigation (`dfir/prompt-book.md`)
**Focus:** Full-spectrum forensic analysis triggered by DLP alerts

40 prompts across 10 investigation phases covering:
- DLP alert triage and initial scoping
- Data exfiltration path analysis
- Insider risk signal correlation
- Device posture and endpoint forensics
- Identity and sign-in risk analysis
- Evidence chain construction
- Timeline reconstruction

**Best for:** DFIR analysts, incident response teams, forensic investigations

**Validation Status:** [ASPIRATIONAL] — structured for SC + Purview with investigative depth requiring analyst judgment

---

## 🗺️ How to Use These Prompt Books

### For Internal CSAs and Champions
1. **Sales enablement:** Use workshop prompts for customer demos and talking points
2. **Customer enablement:** Adapt audit, MIP, and operations prompts for client workshops
3. **Playbooks:** Use incident response and operations guidance for customer implementation
4. **Reference:** Cite validated patterns when discussing SC + Purview capabilities

### For Customers (via CSA)
1. **Immediate value:** Start with audit and workshop "quick demo" prompts
2. **Investigation:** Use audit prompts for incident response workflows
3. **Strategy:** Use MIP and operations prompts for label and policy planning
4. **Ongoing:** Adopt executive and operations prompts for quarterly governance

### For Internal Security Teams
1. **Audit and compliance:** Use audit prompts for regulatory investigation
2. **Policy management:** Use operations prompts for DLP and label maintenance
3. **Leadership reporting:** Use executive prompts for board and CISO briefings
4. **Continuous monitoring:** Use workshop "KPI dashboard" prompt for metrics

---

## Validation Markers

Every prompt is marked with status:

- **[VALIDATED]**: Tested against real Security Copilot + Purview environment. Reliable output, customer-ready.
- **[ASPIRATIONAL]**: Proven conceptually, requires cross-plugin coordination or external data. Recommend testing in customer environment before deploying.

Look for these markers in the "Tags and Patterns" section of each prompt book.

---

## Real SC Purview Capabilities Referenced

These prompt books leverage:

**Core Capabilities:**
- Get Data Risk Summary
- Get User Risk Summary
- Summarize Purview Alert
- Triage Purview Alerts
- Zoom Into Purview Data Risk
- Zoom Into Purview User Risk

**Embedded Capabilities:**
- DLP alert summarization
- IRM alert/user summarization
- DSPM analysis
- Communication Compliance summarization
- eDiscovery summarization
- Policy insights
- Activity explorer (preview)

All prompts are written for realistic, achievable SC capabilities. No prompts require features not yet available.

---

## Important Notes

### For CSAs and Presenters
- Customize prompts to your customer's specific data/policies
- Always test prompts in customer environment before live demo
- Have fallback slides ready in case SC connectivity issues
- Adjust data categories and timelines to match customer scope
- Include customer success stories/examples where possible

### For Implementation Teams
- Validate all prompts work with your Purview configuration
- Test auto-labeling and DLP rules in pilot before org-wide rollout
- Adapt compliance requirements to your jurisdiction (GDPR != CCPA)
- Document your custom policies and label taxonomy
- Establish governance before deploying prompts to end users

### For Leadership
- These prompts support decision-making, don't replace human judgment
- Success depends on Purview data quality and audit log retention
- Invest in training to drive adoption of label and policy frameworks
- Review prompts quarterly; update as business and regulatory needs evolve

---

## Version and Maintenance

- **Created:** April 2026
- **Last Updated:** April 2026
- **Maintainer:** CSA / Security Copilot Champion
- **Review Cycle:** Quarterly (Q2, Q3, Q4)

Next review: July 2026

### Update Process
1. Test prompt against latest SC + Purview release
2. Validate output quality and relevance
3. Incorporate customer feedback and lessons learned
4. Update validation status and tags
5. Communicate changes to CSA team

---

## Feedback and Contributions

Found an issue? Have a great prompt to add?

- Report issues to: [security-copilot-team@microsoft.com]
- Suggest new prompts with: [context, purpose, expected output]
- Share customer wins that validate prompts
- Document lessons learned from deployments

This library grows with your contributions.

---

## Related Resources

- [Microsoft Purview Data Security Documentation](https://learn.microsoft.com/purview/data-security)
- [Security Copilot for Purview Plugins](https://learn.microsoft.com/security-copilot)
- [DLP Policy Best Practices](https://learn.microsoft.com/microsoft-365/compliance/dlp)
- [Sensitivity Labels Guide](https://learn.microsoft.com/microsoft-365/compliance/sensitivity-labels)
- [Audit Log Retention Policies](https://learn.microsoft.com/microsoft-365/compliance/retention-policies)

---

## License and Usage

These prompts are internal Microsoft resources for CSA use and customer enablement. 

Permitted: Customer sharing, internal documentation, CSA training
Not permitted: External publication, third-party libraries, competitive products

