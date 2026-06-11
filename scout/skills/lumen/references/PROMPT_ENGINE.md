# Prompt Engine — patterns, taxonomy, principles, output

How to generate Security Copilot prompts from scratch for any Purview data-security use case.
Never rely on canned templates — synthesize by reasoning over capabilities + the items below.

## Step 1 — Classify into a taxonomy category (9)
1. **Triage** — assess severity, prioritize, route. Output: severity + recommended action.
2. **Investigation** — deep-dive, correlate, reconstruct timeline, find root cause. Output: timeline + evidence + confidence.
3. **Summarization** — condense for non-experts. Output: plain-language summary + business impact.
4. **Reporting** — formal/audit-ready reports. Output: formatted report w/ metadata, timeline, remediation.
5. **Enrichment** — add context (role, tenure, history) to raw alerts.
6. **Hunting** — proactively search for indicators/anomalies. Output: ranked findings + frequency + confidence.
7. **Advisory** — recommend policy tuning / risk mitigation. Output: ranked recommendations + effort + metrics.
8. **Operational** — assess control readiness/health/config. Output: audit results + gaps.
9. **Workshop/Demo** — demonstrate capabilities. Output: narrated walkthrough + talking points.

## Step 2 — Choose design pattern(s) (10)
1. **Summarizer** — Summarize [data] from [source] for [audience] focusing on [aspect]; include [sections].
2. **Investigator** — Investigate [alert] via [sources]; look for [indicators]; output [format]; include confidence + sources.
3. **Triager** — Given [alert], assess severity by [criteria]; recommend [action]; include scoring rationale.
4. **Explainer** — Explain [finding] for a [persona]; include [context]; avoid/define jargon.
5. **Reporter** — Generate [report type] covering [scope] for [audience]; include [sections]; format for [medium].
6. **Advisor** — From [current state], recommend [improvement] given [constraints]; include effort + success metrics.
7. **Hunter** — Search [sources] for [pattern] over [range]; flag [criteria]; rank by [method]; include frequency + confidence.
8. **Correlator** — Correlate [signal A] with [signal B] for [entity] over [range]; flag [patterns]; give confidence + alternatives.
9. **Operation Anchor** — Analyze [source] for [entity] for these exact ops: [EXACT_OPERATION_NAMES]; for each give [format]. Use when SC mis-maps NL to Unified Audit Log operations.
10. **SIT Anchor** — Show [alert type] where SIT "[EXACT_SIT_NAME]" was detected at [confidence] in [range]. Use when data-type precision matters.

## Step 3 — Apply all 8 design principles
1. Specific Purview sources (DLP matches, IRM indicators, exact audit ops) — never "investigate the user".
2. Name persona (who runs) + audience (who reads).
3. Explicit output format (timeline/table/ranked list/narrative).
4. Bounded time range — never open-ended.
5. Exact identifiers (full policy names, UPNs, exact SIT names, departments).
6. Ask for confidence + caveats (investigation/hunting).
7. Request source attribution for verification.
8. Make outputs chainable (clear entities/identifiers for the next step).

## Step 4 — Output format (use for every use case)
```
### Use Case Analysis
Customer Use Case: …
Taxonomy Category: …
Pattern(s) Used: …
SC Capabilities Required: … (of the 6 + any agent)
Validation Status: [VALIDATED] | [ASPIRATIONAL]

### Generated Prompt(s)
Prompt Title / Who Runs This / SC Experience: Standalone | Embedded | Promptbook
```<the actual copy-paste-ready prompt>```
(for promptbooks: number each step; note SC capability + what feeds the next step)

### Expected Output / Limitations / Tips
```

## Reference data
- Exact audit operations: [AUDIT_LOG_OPERATIONS.md](AUDIT_LOG_OPERATIONS.md)
- Exact SIT names: [SENSITIVE_INFO_TYPES.md](SENSITIVE_INFO_TYPES.md)

## Guardrails
- Map every prompt to one of the 6 capabilities; flag anything else `[ASPIRATIONAL]`.
- Flag SCU cost for multi-step promptbooks.
- Promptbook parameters: `<AngleBrackets>`, no spaces.
- Fictitious example data only (Contoso/Fabrikam).
