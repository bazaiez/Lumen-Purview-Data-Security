# Promptbook YAML Spec

A portable, reviewable representation of a Security Copilot promptbook. Use it for version
control and review; transcribe into the in-product promptbook builder for execution.

## Schema
```yaml
promptbook:
  name: string            # required, short title
  description: string     # required, one sentence on purpose + Purview scope
  persona: string         # who runs this (SOC Analyst, Compliance Officer, CSA…)
  domain: string          # DLP | IRM | DSPM | MIP | Audit | DSI  (optional)
  validation: string      # VALIDATED | ASPIRATIONAL
  scu_note: string        # optional cost note for long promptbooks
  parameters:             # reusable inputs, each referenced as <Name> in prompts
    - name: <ParameterName>   # angle brackets, NO spaces
      description: string
      example: string          # optional, fictitious
  steps:
    - title: string
      capability: string  # one of the 6 Purview plugin capabilities (or named agent)
      prompt: string      # the copy-paste prompt; uses <Parameters>
      feeds: string       # optional: which later step consumes this output
```

## Field rules
- **parameters[].name** — `<PascalCaseNoSpaces>`. Every `<token>` used in a prompt must be
  declared here exactly once.
- **steps[].capability** — must be one of: Get Data Risk Summary, Get User Risk Summary,
  Summarize Purview Alert, Triage Purview Alerts, Zoom Into Purview Data Risk, Zoom Into
  Purview User Risk — or a named agent (Microsoft Purview Triage Agent in DLP/IRM, Data
  Security Posture Agent). Anything else ⇒ set `validation: ASPIRATIONAL`.
- **steps[].prompt** — follow the 8 design principles (specific sources, persona+audience,
  explicit format, time range, exact identifiers, confidence, attribution, chainable).
- Prefer exact audit-operation names and exact SIT names over natural language.

## Mapping to the SC promptbook builder
| YAML field | Builder field |
|---|---|
| `promptbook.name` | Promptbook name |
| `promptbook.description` | Description |
| `parameters[]` | "Specify any required input" — add each `<Name>` (no spaces) |
| each `steps[].prompt` | A prompt added in order |
| `steps[].title` | Use as the prompt's purpose note |

Builder reference: Microsoft Learn "Create and manage promptbooks" / "Build promptbooks".

## Validation checklist
- [ ] name + description present
- [ ] every `<Parameter>` declared once, no spaces
- [ ] every step capability is real (else ASPIRATIONAL)
- [ ] steps are ordered and chain logically
- [ ] SCU note added if > 5 steps
- [ ] fictitious example data only
