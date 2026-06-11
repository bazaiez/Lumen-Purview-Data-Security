# /generate-prompt — Security Copilot Prompt Generator Skill

**Version:** 1.0
**Type:** Claude Code Skill
**Status:** Production Ready

---

## Purpose

This skill turns any natural language customer use case into a complete, production-ready Security Copilot prompt or promptbook. It is the primary interface to the Prompt Generator Agent.

---

## Usage

```
/generate-prompt <use case description>
```

### Examples

```
/generate-prompt Investigate a departing employee who downloaded 500 sensitive files in their last week

/generate-prompt Our compliance team needs weekly DLP summary reports for the board

/generate-prompt A customer wants to find all exposed credit card data across SharePoint and OneDrive

/generate-prompt Junior SOC analyst needs help triaging 200 DLP alerts per day for financial data policies

/generate-prompt HR flagged an insider risk case — employee sharing trade secrets via personal email over 90 days
```

---

## What It Does

1. **Analyzes** the use case to identify the Purview domain (DLP, IRM, DSPM, MIP, Audit)
2. **Classifies** into one or more of 9 taxonomy categories (Triage, Investigation, Summarization, Reporting, Enrichment, Hunting, Advisory, Operational, Workshop)
3. **Selects** the optimal prompt design pattern(s) from 10 proven patterns
4. **Maps** to validated SC capabilities (the 6 Purview plugin capabilities + 3 agents)
5. **Generates** a complete prompt or multi-step promptbook with:
   - Copy-paste-ready prompt text
   - Expected output description
   - Relevant limitations and caveats
   - Tips for best results
   - Quality checklist
   - Prompt variations for different scenarios
   - Follow-up prompts for chaining
   - Error recovery prompts for common SC failures
6. **Validates** each prompt as [VALIDATED] or [ASPIRATIONAL]

---

## Clarification Questions

If the use case is ambiguous, the skill will ask:

- **Who** is the target persona? (SOC analyst, compliance officer, CISO, CSA, admin)
- **Which** Purview area? (DLP, IRM, DSPM, MIP, Audit, multiple)
- **What data** do they have? (alert IDs, user UPNs, policy names, time ranges)
- **What output** do they need? (triage decision, investigation report, executive summary, compliance doc)
- **What audience** receives the output? (technical analyst, non-technical manager, regulator)

---

## Output Format

The skill outputs the full structured format defined in the Prompt Generator Agent:

1. **Use Case Analysis** — classification, patterns, capabilities, validation status
2. **Generated Prompt(s)** — the actual SC prompts, copy-paste ready
3. **Expected Output** — what SC will return
4. **Limitations and Caveats** — honest assessment of gaps
5. **Tips for Best Results** — optimization guidance
6. **Quality Checklist** — verification against 8 design principles
7. **Variations** — alternative versions for different scopes/audiences
8. **Follow-Up Prompts** — natural next steps for chaining
9. **Error Recovery** — fallback prompts if SC misbehaves

---

## Integration with Repository

Generated prompts can be saved to the appropriate prompt book:

| Domain | Save To |
|--------|---------|
| DLP | `prompts/dlp/prompt-book.md` |
| IRM | `prompts/irm/prompt-book.md` |
| DSPM | `prompts/dspm/prompt-book.md` |
| Audit | `prompts/audit/prompt-book.md` |
| MIP | `prompts/mip/prompt-book.md` |
| Executive/Reporting | `prompts/executive/prompt-book.md` |
| Workshop/Demo | `prompts/workshop/prompt-book.md` |
| Operations | `prompts/operations/prompt-book.md` |

After generating, you can ask:

```
Save this prompt to the DLP prompt book
```

---

## Quality Guarantees

Every generated prompt passes these checks:

- Uses ONLY validated SC capabilities (no fictional features)
- Follows all 8 prompt design principles
- Includes relevant limitations (no false promises)
- Uses exact SIT names and audit log operation names (no approximations)
- Designed to chain (output feeds next prompt)
- Marked with honest validation status
- Includes error recovery for common SC failures
