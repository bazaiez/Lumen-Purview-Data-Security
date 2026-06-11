# Demo Engine — customer demos & workshops

Help a CSA design compelling, honest, technically accurate Security Copilot + Purview demos.
Design to the customer's real problems, not a generic product tour. Use fictitious data only.

## Step 1 — Understand the customer (ask if not provided)
Industry · org size (alert volume, SCU budget) · Purview maturity · top pain point · who's in the
room (SOC/CISO/compliance/IT) · existing tools (Defender/Sentinel?) · stage (evaluate/deploy/optimize).
Ask: industry? audience? biggest data-security pain? new to Purview? time available?

## Step 2 — Pick an industry scenario (fictitious)
- **Financial:** proprietary models to personal email; PCI exposure audit; 500 DLP alerts/day.
  SITs: "Credit Card Number", "U.S. Bank Account Number", "International Banking Account Number (IBAN)".
- **Healthcare:** patient records mis-shared in Teams; departing physician downloads patient lists; PHI audit.
  SITs: "U.S. Health Insurance Claim Number (HICN)", custom PHI.
- **Technology:** source code + API keys to personal GitHub; roadmap to external partner; departing engineer bulk download.
  SITs: "Azure Storage Account Key", "Azure AD Client Secret", "AWS Access Key".
- **Government:** classified docs to personal email; over-clearance access; PII compliance audit.
- **Retail:** excessive customer PII access; segments shared with agency; seasonal-worker risk.
- **Manufacturing/Energy:** designs to supplier; trade-secret over-access; board docs forwarded.

## Step 3 — Build the demo script (timed acts)
- **Opening (2–3m):** set the fictional-but-realistic scene; why it matters to this industry.
- **Act 1 — The Alert (3–5m):** triage/summarize the alert. "Manually this takes an analyst X min."
- **Act 2 — The Investigation (5–10m):** user risk profile → data risk → activity timeline (exact
  operations) → DLP+IRM correlation. "SC correlates across consoles you'd normally switch between."
- **Act 3 — The Decision (3–5m):** generate investigation summary / exec brief. "Escalate or close in minutes."
- **Act 4 — The Value (2–3m):** quantify time saved / coverage; what's next (tuning, monitoring).
- **Closing — Honest limitations (2–3m):** what SC cannot do; frame as human-in-the-loop; SCU cost.

## Step 4 — Prep the hard Q&A (accurate answers)
- "Can SC auto-close/remediate?" → No (analyze + recommend). Exception: DLP Triage Agent can send
  Teams remediation reminders (preview) to a file's last editor — it still does not close the alert.
- "SCU cost?" → SCUs = Security Compute Units; each prompt/agent call consumes them; budget by
  daily alert volume × prompts per investigation. M365 E5 now includes default SCU capacity (Nov 2025).
- "GCC/sovereign?" → Commercial only; FedRAMP High GA (Azure Commercial); GCC not yet.
- "Does SC read our data?" → Respects Purview RBAC; uses metadata/classifications/policy matches,
  NOT raw file contents.
- "GA or preview?" → DLP & IRM Triage Agents GA; Data Security Posture Agent preview.

## Step 5 — Hands-on lab (workshops)
Provide 3–5 guided exercises mirroring the scenario, each with: objective, the SC prompt, expected
output, and a discussion question. Always seed with synthetic data — never real customer data.

## Deliverable hand-off (Scout)
Offer to produce the demo script as a **.pptx/.docx** via the report-deliverable-generator skill,
and the live prompt sequence as an importable promptbook via promptbook-yaml-exporter.

## Guardrails
Every demo prompt maps to a real capability; include the limitations act; fictitious data only;
timing realistic; never imply capabilities SC lacks.
