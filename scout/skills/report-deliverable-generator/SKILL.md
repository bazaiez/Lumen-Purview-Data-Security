---
name: report-deliverable-generator
description: >
  Turn Security Copilot + Purview output (investigation playbooks, executive summaries, demo
  scripts, prompt books, readiness assessments) into a polished, branded deliverable FILE —
  Word (.docx), PowerPoint (.pptx), or Excel (.xlsx). Use this whenever the user wants a
  data-security report/playbook/briefing AS A DOCUMENT, asks to "export", "make a deck",
  "write up", "send to the customer/CISO", or wants a leave-behind. Triggers: investigation
  report, executive briefing, board report, demo deck, customer leave-behind, export to Word/
  PowerPoint/Excel, generate document, prompt-book spreadsheet. Pairs with the lumen
  skill (which produces the content).
---

# Report Deliverable Generator

Convert security-data content into a customer-ready file using Scout's document skills.
This skill decides the format and structure, then hands the actual file creation to the
**docx**, **pptx**, or **xlsx** skills.

> Sensitivity: outputs may include content read from M365 under a sensitivity label. Tell the
> user the session's sensitivity level before producing a shareable file, and never write
> classified content to an unprotected destination. Use **fictitious** data in samples.

## Choose the format
| Deliverable | Format | Skill to invoke |
|---|---|---|
| Investigation playbook / report, incident write-up, runbook | `.docx` | docx |
| Executive / board briefing, demo deck, workshop slides | `.pptx` | pptx |
| Prompt book, alert/triage matrix, validation matrix, SCU estimate | `.xlsx` | xlsx |

## Workflow
1. **Confirm scope** — what content, which audience (analyst/CISO/compliance), which format.
2. **Sensitivity check** — state the session sensitivity level; confirm before producing a
   shareable file if it contains data from M365.
3. **Structure the content** for the audience using the templates below.
4. **Invoke the matching Scout skill** (docx/pptx/xlsx) to create the file in the workspace.
5. **Verify** the file exists and opens; report the path. For images/diagrams, render inline.
6. **Add a footer note**: "Generated draft — verify against your tenant; mark sensitivity label
   before sharing externally."

## Structure templates
**Investigation report (.docx):** Title + incident ID · Executive summary (3–5 lines) ·
Severity & scope · Timeline (table) · Evidence (with Purview source attribution) · Findings ·
Remediation (immediate/short/long) · Escalations · **Limitations** (what SC could not
determine) · Appendix: SC prompts used.

**Executive briefing (.pptx):** Situation · Business impact · What we found · Actions taken /
recommended · Risk reduced · Cost (SCU) note · Next steps. Keep ~7 slides; lead with impact.

**Prompt book (.xlsx):** columns = Prompt Title · Persona · Taxonomy · Pattern · Capability ·
Validation Status · Prompt Text · Expected Output · Limitations. One row per prompt; freeze
header; color-code validation status.

## Guardrails
- Never assert SC capabilities beyond the six + three agents (see lumen GROUND_TRUTH).
- Always include a Limitations section/slide.
- Fictitious names only in examples; real customer data only with explicit user confirmation
  and never to an unprotected destination.

## Post-Run Reflection
Did I confirm sensitivity before a shareable file? Is the structure audience-appropriate? Does
it carry a Limitations section and a draft/verify footer? Fix before delivering.
