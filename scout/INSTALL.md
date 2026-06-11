# Lumen — Microsoft Scout integration

This `scout/` layer makes Lumen usable as native **Microsoft Scout skills**, in addition
to the existing Claude Code experience (`CLAUDE.md`, untouched). Four skills are provided:

| Skill | What it does |
|---|---|
| `lumen` | Main expert: answer, generate prompts, build playbooks, design demos, review quality. |
| `promptbook-yaml-exporter` | Package prompts into a portable promptbook spec (YAML) + builder guide. |
| `report-deliverable-generator` | Turn output into `.docx` / `.pptx` / `.xlsx` via Scout's document skills. |
| `preflight-readiness` | Prerequisite + go/no-go check before a demo, pilot, or deployment. |

## What "Scout-compatible" means here
Each skill is a directory with a `SKILL.md` (YAML frontmatter `name` + a "pushy" `description`
with trigger phrases; body ≤ 500 lines) and a `references/` folder for heavy content loaded on
demand. This matches the Scout skill convention (golden master: the `acr` skill).

## Install (make the skills active in Scout)
Copy the skill folders into your Scout custom-skills directory, then start a fresh session.

```powershell
$src = "<repo-root>\scout\skills"
$dst = "$env:USERPROFILE\.copilot\m-skills"
Copy-Item "$src\lumen"               $dst -Recurse -Force
Copy-Item "$src\promptbook-yaml-exporter"    $dst -Recurse -Force
Copy-Item "$src\report-deliverable-generator" $dst -Recurse -Force
Copy-Item "$src\preflight-readiness"         $dst -Recurse -Force
```

Verify in a new Scout session by asking (without naming the skill):
- "Can Security Copilot detect USB file transfers?"  → `lumen` should answer.
- "Generate SC prompts for departing-employee monitoring." → `lumen`.
- "Turn that into a promptbook." → `promptbook-yaml-exporter`.
- "Are we ready to demo Purview agents next week?" → `preflight-readiness`.
- "Make a CISO briefing deck from this." → `report-deliverable-generator`.

## Update after changes
Re-run the copy commands above and start a new session (skills load at session start).

## Accuracy
Ground truth in `skills/lumen/references/GROUND_TRUTH.md` is verified against Microsoft
Learn (June 2026): 6 plugin capabilities, **3** agents (DLP & IRM Triage = GA; Data Security
Posture Agent = preview, DSPM + DSI), SCU = **Security Compute Units**, commercial-cloud-only,
and the no-write-back rule with the DLP Teams-reminder (preview) exception. Re-validate quarterly.
