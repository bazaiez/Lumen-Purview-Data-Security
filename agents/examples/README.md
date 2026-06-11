# Agent Scenario Examples

Concrete walkthroughs showing how the 6-agent system handles real scenarios.

## Example 1: DLP Alert Investigation (Healthcare)

**User input:** "Generate DLP investigation prompts for a healthcare customer dealing with PHI exfiltration alerts."

**Agent flow:**
1. **Orchestrator** receives request, identifies: Prompt Generation + industry-specific
2. **Knowledge Agent** confirms SC can triage DLP alerts and zoom into data risk; confirms PHI-related SITs ("U.S. Health Insurance Claim Number (HICN)")
3. **Prompt Generator Agent** generates healthcare-specific prompts using SIT Anchor pattern with exact HIPAA SIT names
4. **Investigation Agent** structures prompts into incident response workflow with HIPAA escalation criteria
5. **Quality Reviewer Agent** validates all prompts against real SC capabilities, confirms [VALIDATED] status
6. **Output:** Promptbook sequence + investigation playbook + healthcare-specific limitations

---

## Example 2: IRM Case Triage Demo (Financial Services)

**User input:** "I have a banking customer's SOC team meeting Thursday. Need to demo IRM case management."

**Agent flow:**
1. **Orchestrator** receives request, identifies: Demo Preparation + Prompt Generation
2. **Knowledge Agent** confirms user risk summary and zoom capabilities; flags IRM Triage Agent limitation (SharePoint only in preview)
3. **Demo Architect Agent** designs financial services SOC demo with trading desk insider risk scenario
4. **Prompt Generator Agent** generates risk-scoring prompts with financial context
5. **Quality Reviewer Agent** validates demo prompts won't reference fictional capabilities; ensures limitation discussion included
6. **Output:** Demo script + SC prompt sequences + talking points + Q&A prep + hands-on lab

---

## Example 3: Cross-Signal Investigation (Technology)

**User input:** "Investigate: departing engineer triggered both DLP and IRM alerts, bulk downloaded source code last week."

**Agent flow:**
1. **Orchestrator** receives request, identifies: Active Incident → Investigation Agent primary
2. **Knowledge Agent** confirms cross-signal correlation requires data sharing enabled between DLP and IRM
3. **Investigation Agent** builds 5-phase playbook with SC prompts at every step, uses Operation Anchor pattern with exact audit operations (FileDownloaded, FileSyncDownloadedFull, FileCopied, FileCreatedOnRemovableMedia)
4. **Prompt Generator Agent** generates additional prompts for credential scanning (Data Security Posture Agent in DSI) since source code may contain API keys
5. **Quality Reviewer Agent** flags that audit log lag (24-48 hours, a Purview UAL platform characteristic) may affect timeline accuracy; confirms all prompts are [VALIDATED]
6. **Output:** Investigation playbook + SC prompts + decision trees + remediation actions + escalation matrix + evidence preservation checklist

---

## Example 4: Complex Multi-Domain Request

**User input:** "Retail customer, 1000 DLP alerts/day, 5-person SOC, CCPA compliance concerns. Need: triage prompts + investigation playbook + quarterly reporting + half-day workshop."

**Agent flow:**
1. **Orchestrator** plans multi-agent pipeline:
   - Knowledge Agent → capability check for retail/CCPA
   - Prompt Generator → DLP triage prompts + reporting prompts
   - Investigation Agent → incident response playbook
   - Demo Architect → workshop design with hands-on labs
   - Quality Reviewer → validate everything
2. Each agent executes its part
3. **Orchestrator** synthesizes into unified customer package
4. **Output:** Complete package with all four deliverables + cross-references between them

---

**See also:** [AGENT-HUB.md](../AGENT-HUB.md) for how to get started, [architecture.md](../architecture.md) for the full design.
