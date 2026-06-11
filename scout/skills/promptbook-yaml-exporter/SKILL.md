---
name: promptbook-yaml-exporter
description: >
  Convert a set of Security Copilot prompts into a structured, version-controllable promptbook
  specification (YAML) plus a step-by-step guide for the Security Copilot promptbook builder.
  Use this whenever the user wants to TURN prompts INTO a promptbook, package a multi-step SC
  workflow, version-control prompts, or "export/build a promptbook" for Microsoft Purview data
  security. Triggers: promptbook, build a promptbook, export promptbook, promptbook YAML,
  package these prompts, multi-step SC workflow, promptbook parameters. Pairs with the
  lumen skill (which generates the prompts).
---

# Promptbook YAML Exporter

Package generated Security Copilot prompts into a portable promptbook spec (YAML) and a
builder guide. The YAML is for version control and review; Security Copilot promptbooks are
assembled in the in-product **promptbook builder**, so the output also maps each step to the
builder fields.

> Honesty rule: do NOT claim Security Copilot auto-imports this YAML. It is a portable,
> reviewable spec. The accompanying guide tells the user exactly what to paste into the
> "Build a promptbook" experience. If your tenant supports a direct import path, the same
> fields apply.

## Workflow
1. **Collect inputs** — the ordered prompts (from the lumen skill), a promptbook name,
   description, intended persona, and the Purview capability each step uses.
2. **Extract parameters** — find values that should be reusable inputs (UPNs, policy names,
   time ranges, SIT names, alert IDs). Render them as `<ParameterName>` — **angle brackets, no
   spaces** (Security Copilot's documented syntax). List each parameter once at the top.
3. **Order & chain** — ensure each step's output feeds the next; note the dependency.
4. **Emit YAML** following the spec in [PROMPTBOOK_SPEC.md](references/PROMPTBOOK_SPEC.md).
5. **Emit the builder guide** — numbered steps mapping YAML → the SC promptbook builder fields
   (Name, Description, each Prompt, and "Specify any required input" parameters).
6. **Validate** — every prompt maps to a real Purview capability (see lumen
   GROUND_TRUTH); flag SCU cost for long promptbooks; mark `[VALIDATED]`/`[ASPIRATIONAL]`.
7. **Save** the `.yaml` to the workspace and offer to also produce a `.docx` runbook via the
   report-deliverable-generator skill.

## Minimal example
```yaml
promptbook:
  name: Departing Employee Risk Review
  description: Assess data-exfiltration risk for a departing employee using Purview IRM + DLP.
  persona: SOC Analyst
  parameters:
    - name: <UserUPN>
      description: User principal name of the departing employee
    - name: <TimeRange>
      description: e.g. last 30 days
  steps:
    - title: User risk overview
      capability: Get User Risk Summary
      prompt: >
        Summarize the insider-risk profile for <UserUPN> over <TimeRange>: risk level,
        exfiltration indicators, and sequential activities. Cite the Purview source for each.
    - title: Exfiltration activity deep dive
      capability: Zoom Into Purview User Risk
      prompt: >
        For <UserUPN> over <TimeRange>, show these audit operations: FileDownloaded,
        FileSyncDownloadedFull, FileCopiedToRemovableMedia, FileUploadedToCloud. For each:
        timestamp, file name, and whether a DLP policy matched. Output a timeline; include
        confidence and what would change your assessment.
```

## References
- [PROMPTBOOK_SPEC.md](references/PROMPTBOOK_SPEC.md) — full YAML schema, field rules, builder mapping.

## Post-Run Reflection
After emitting: did every step map to a real capability? Are all parameters `<NoSpaces>`? Does
each step chain to the next? Did I avoid claiming auto-import? Fix before delivering.
