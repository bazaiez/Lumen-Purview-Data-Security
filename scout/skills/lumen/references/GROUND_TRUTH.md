# Ground Truth — Security Copilot + Purview Data Security

Authoritative capability/limitation reference. Verified against Microsoft Learn (June 2026).
This overrides any older claim elsewhere in the repo. When a limitation here contradicts a
claimed capability, the limitation wins.

## How Security Copilot works
- AI assistant that processes natural-language prompts via a **plugin** architecture.
- The **Purview plugin** exposes the 6 capabilities below; enabled per-tenant in the SC admin center.
- Context lives within a single **session**; new sessions start fresh; long sessions lose early context.
- SC respects **Purview RBAC** — users see only what they're permitted to.
- Parameters use `<AngleBrackets>` with **no spaces**.

## The 6 Purview plugin capabilities (standalone) — the ONLY six

### 1. Get Data Risk Summary
- Fetches MIP + DLP; summarizes data risk for an incident or DLP alert.
- Returns: sensitivity labels, DLP policy matches, classification, risk attributes.
- Cannot: read file contents; judge whether sharing was authorized.

### 2. Get User Risk Summary
- Summarizes a user's IRM risk profile.
- Returns: risk level, exfiltration indicators, sequential activities, activity types.
- Cannot: determine intent or motivation.

### 3. Summarize Purview Alert
- Details of a specific DLP or IRM alert with context.
- Returns: policy name, user, severity, triggered rule, matched content type.
- Cannot: access alerts beyond retention; read matched content.

### 4. Triage Purview Alerts
- Retrieves top/recent DLP alerts, ranks/groups by risk.
- Cannot: close or dismiss alerts (no write-back).

### 5. Zoom Into Purview Data Risk
- Detailed MIP + DLP attributes and risk for specific data.
- Returns: classification detail, DLP rule-match detail, label history.
- Cannot: show file content or custom-SIT regex internals.

### 6. Zoom Into Purview User Risk
- Deep dive into a user's activities, operations, exfiltration, sequences, anomalies.
- Returns: activity timeline, operation types, exfiltration patterns, sequence chains.
- Cannot: determine intent; assess manager approval; compute custom baselines.

> RULE: every generated prompt maps to one or more of these six. Otherwise mark `[ASPIRATIONAL]`.

## Security Copilot agents in Purview — THREE agents

### Microsoft Purview Triage Agent in DLP  (DLP Triage Agent) — GA
- Auto-categorizes DLP alerts: Needs attention / Less urgent / Not categorized.
- Active-mode policies only (NOT simulation/audit mode). Triages files ≤ **2 MB**.
- Alerts > 30 days before enablement out of scope (non-rolling window).
- Native Purview DLP only (not custom DLP solutions).
- **Teams remediation reminders (preview):** for SharePoint/OneDrive alerts triaged "Needs
  attention", can send a Teams chat to the file's last-modified user asking them to remove the
  sensitive content. This is the only outbound action — it does NOT close the alert.

### Microsoft Purview Triage Agent in Insider Risk Management  (IRM Triage Agent) — GA
- Auto-categorizes IRM alerts into the same categories.
- **Preview-era residual:** analyzes SharePoint **file content only** — not email/device
  activities. Alerts with only email or endpoint activity aren't analyzed.
- Risk scoring is model-based; role baselines may be thin for small teams / new hires.

### Data Security Posture Agent — Preview
- ONE agent surfaced in TWO Purview solutions:
  - **DSPM:** natural-language data discovery ("where is our sensitive data?").
  - **Data Security Investigations (DSI):** tenant-wide **credential scanning**, AI risk
    assessments with confidence scores, Kanban task board, KQL via Data Explorer.
- Deploying the Posture Agent once makes it available in both. There is **no separate DSI agent**.
- Recommendations only — cannot remediate. Scan coverage depends on connectors.

## Embedded experiences (in the Purview portal)
- "Summarize with Copilot" on DLP and IRM alerts.
- Policy insights; DSPM posture prompts.
- Communication Compliance summarization; eDiscovery summarization.
- **Activity Explorer + Copilot (preview)** — NL prompts to generate filters / insights.
- **Global Copilot entry point** — context-aware Copilot button in the Purview suite header.

## Capacity, licensing, cloud
- **SCUs = Security Compute Units.** Provisioned (monthly) + overage (on-demand, GA). Agents
  and prompts consume SCUs per run based on volume/complexity; track via in-product usage tool.
- **Nov 2025:** SC included for **Microsoft 365 E5** with default SCU capacity auto-provisioned.
- Agents support a dedicated **Microsoft Entra Agent ID** (recommended) to separate agent
  actions from individual user identity.
- **Commercial cloud only.** FedRAMP High GA (Azure Commercial). **GCC not yet** available.

## Known limitations (complete)
### Data access
- Works with metadata/classifications/policy matches — **not raw file content**.
- Custom SIT regex internals may be opaque.
- Cannot read Teams/email bodies for intent.
- Endpoint activity outside Purview DLP scope is invisible.

### Data freshness
| Source | Lag | Impact |
|---|---|---|
| DLP alerts | 15–30 min | Near real-time triage |
| IRM alerts | Near real-time | Good for active threats |
| Purview Unified Audit Log | Can lag (UAL platform trait) | Timeline may miss most-recent events |
| DSPM scans | Days–weeks | Posture reflects scan date, not now |

### Analytical
- Cannot determine **intent**; business context invisible; no native confidence scores.
- Cross-signal (DLP+IRM) correlation requires data sharing enabled.
- "Summarize" may omit detail on complex data; long sessions drift.

### Operational
- **No policy/alert write-back** (cannot close alerts or change policies). Exception: DLP
  Triage Agent Teams reminders (preview).
- No conditional branching in promptbooks; complex objects may not pass cleanly between steps.
- Responses vary between sessions.

### Deployment
- Commercial cloud only (see above). Requires SCU capacity + SC access assigned.
- Purview plugin must be explicitly enabled per-tenant.

## Prerequisites by scenario
| Scenario | Prerequisites |
|---|---|
| DLP alert triage | Purview plugin enabled, DLP Admin role, **active-mode** DLP policies |
| IRM investigation | Purview plugin + IRM, IRM Analyst/Admin role, data sharing for cross-signal |
| DSPM posture queries | Posture Agent deployed, connectors configured, recent scan |
| DLP Triage Agent | Active-mode policies, agent enabled, (optional) Entra Agent ID + Teams reminders |
| IRM Triage Agent | IRM policies active, agent enabled (SharePoint file content only) |
| Cross-signal correlation | DLP + IRM data sharing enabled in Purview settings |

## Sources (Microsoft Learn)
- copilot-in-purview-overview · copilot-in-purview-promptbooks
- copilot-in-purview-agents-overview · copilot-in-purview-triage-dlp-agent-get-started
- data-security-investigations-posture-agent · copilot-in-purview-posture-agent-get-started
- security-compute-units-capacity · build-promptbooks · whats-new-copilot-security
