# Lumen — Purview Data Security: Use Case Catalog

## Overview

This directory contains a comprehensive catalog of real-world use cases for Security Copilot applied to Microsoft Purview Data Security solutions. These use cases are designed to guide CSAs, security teams, and customers through practical applications of Security Copilot's AI capabilities to improve data security posture, reduce investigation time, and accelerate security operations.

The catalog bridges the gap between general Security Copilot capabilities and specific Purview Data Security scenarios—translating technical capabilities into tangible business outcomes.

## What's Included

**catalog.md** contains 22 use cases organized into 7 categories:

- **DLP (Data Loss Prevention)**: Alert triage, policy investigation, false positive analysis, and policy optimization
- **IRM (Insider Risk Management)**: Alert assessment, case investigation, user activity analysis, and risk scoring
- **DSPM (Data Security Posture Management)**: Risk interpretation and AI-specific scenarios
- **Investigation**: Cross-signal correlation, data leakage hunting, and threat enrichment
- **Sensitivity Labels / MIP**: Label configuration and coverage analysis
- **Reporting**: Executive summaries, incident reports, and compliance reporting
- **Operations**: Readiness assessment and customer engagement preparation

Each use case includes:
- Clear objectives and target personas
- Required inputs and relevant data sources
- Example Security Copilot prompts (ready to copy and use)
- Expected outputs and realistic limitations
- Automation and validation guidance
- Customer value articulation

## How to Use This Catalog

### For CSAs in Customer Engagements

1. **During Discovery**: Review use cases relevant to the customer's environment and pain points. Use them to uncover security operations challenges the customer may not have articulated.

2. **During Demonstrations**: Reference specific use cases that align with customer priorities. Use the provided prompts in live Security Copilot demos to show immediate, tangible value.

3. **In Recommendations**: Build a prioritized use case roadmap for the customer. Start with high-value, low-complexity use cases (e.g., UC-DLP-001, UC-IRM-001) to establish quick wins, then progress to more sophisticated scenarios.

4. **For Enablement**: Share this catalog with customers to help them see the full spectrum of what's possible and to plan their own Security Copilot adoption journey.

### For Security Teams

1. **Operations Planning**: Identify use cases that address current bottlenecks or gaps in your security operations workflow.

2. **Pilot Projects**: Start with automation-friendly use cases (Medium-High potential) that deliver immediate operational improvements without requiring major process changes.

3. **Integration Planning**: Use the "Required Inputs" and "Relevant Purview Data Sources" sections to plan data connections and ensure your environment is configured to support each use case.

### For Security Leadership

Use this catalog to understand the strategic value of Security Copilot investment:
- See which use cases drive cost reduction (fewer manual hours)
- Identify where Security Copilot accelerates response times
- Plan resource allocation based on automation potential
- Build business cases tied to specific operational improvements

## How to Contribute New Use Cases

To add new use cases to this catalog:

1. **Identify a Real Customer Need**: Use cases should address documented challenges in Purview-based security operations.

2. **Follow the Template**: Each use case must include all fields defined in catalog.md (UC ID, Title, Objective, Target Persona, etc.).

3. **Test Prompts in Security Copilot**: Every prompt listed must be validated in a Security Copilot instance to ensure it produces useful, reliable outputs.

4. **Be Honest About Limitations**: Document what Security Copilot cannot do in this scenario. Credibility comes from realistic guidance.

5. **Include a Customer Success Story (Optional)**: If you've deployed a use case with a customer, include a brief note about the outcome and value realized.

6. **Submit a Pull Request**: Add your use case to the appropriate category in catalog.md with full documentation.

### Use Case ID Naming Convention

- **UC-{CATEGORY}-{NUMBER}** (e.g., UC-DLP-001, UC-IRM-002)
- Categories: DLP, IRM, DSPM, INV (Investigation), MIP (Sensitivity Labels), RPT (Reporting), OPS (Operations)
- Numbers start at 001 and increment within each category

## Key Principles

**Practicality**: Every use case has been designed or validated in real customer environments. Prompts and outputs reflect how Security Copilot actually performs.

**Honesty**: We document limitations and edge cases alongside capabilities. Security Copilot is powerful but not a silver bullet.

**Persona-Driven**: Each use case identifies the person who will use it and execute on its output. Different personas need different information.

**Actionability**: Use cases include not just what to ask Security Copilot, but what to do with the results—including validation points and escalation paths.

**Integration-Ready**: All use cases assume standard Purview Data Security configurations. Required inputs and data sources are specific to real Purview capabilities.

## Questions or Feedback?

This catalog is a living document. If you discover new use cases, find limitations with existing ones, or have suggestions for improvement, please contribute feedback through pull requests or issues in this repository.

---

**Last Updated**: April 2026
**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development
