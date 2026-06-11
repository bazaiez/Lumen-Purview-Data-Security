# Readiness Checklist — Security Copilot + Purview

Verify each item (read-only). Record Pass / Fail / Unknown + note. Verified against Microsoft
Learn (June 2026).

## 1. Access & capacity
| Item | How to verify | Blocker? |
|---|---|---|
| Security Copilot access assigned | SC admin center → roles/owners | Yes |
| SCU capacity provisioned (or M365 E5 default capacity active) | SC admin center → usage/capacity | Yes |
| Overage policy set (optional) | SC capacity settings | No |

## 2. Cloud & compliance
| Item | How to verify | Blocker? |
|---|---|---|
| Commercial cloud (not GCC/sovereign) | Tenant cloud type | Yes (SC commercial only) |
| FedRAMP High requirement understood | Azure Commercial FedRAMP High = GA; GCC pending | Context |

## 3. Purview plugin & RBAC
| Item | How to verify | Blocker? |
|---|---|---|
| Purview plugin enabled in SC | SC → Sources/Plugins → Purview | Yes |
| Operator has Purview role for the domain (DLP/IRM/DSPM Admin/Analyst) | Purview roles & scopes | Yes |
| SC respects Purview RBAC understood (sees only permitted data) | n/a — expectation setting | Context |

## 4. Data & policy state
| Item | How to verify | Blocker? |
|---|---|---|
| DLP policies in **active mode** (not simulation) | Purview DLP → policy status | Yes for DLP agent |
| Alerts exist within last 30 days | Purview alerts queue | Risk (sparse triage) |
| DLP+IRM data sharing enabled (if cross-signal expected) | Purview settings → data sharing | Risk |
| Recent DSPM scan completed (if Posture Agent demo) | DSPM → scan status | Risk (preview) |
| IRM scope expectation = SharePoint file content only | Known IRM agent limitation | Risk |

## 5. Agent setup (if demoing agents)
| Item | How to verify | Blocker? |
|---|---|---|
| DLP Triage Agent enabled (GA) | Purview → Copilot agents | Yes (for that demo) |
| IRM Triage Agent enabled (GA) | Purview → Copilot agents | Yes (for that demo) |
| Data Security Posture Agent deployed (preview, DSPM+DSI) | Purview → Copilot agents | Yes (for that demo) |
| Agent runs under Microsoft Entra Agent ID (recommended) | Agent identity setting | Risk |
| Teams remediation reminders configured (optional, preview) | DLP agent setup | No |

## 6. Expectation setting (discuss, not configurable)
- No policy/alert write-back (exception: DLP agent Teams reminders, preview).
- SC reads metadata/classifications/policy matches, not raw file content.
- No native confidence scores; SC cannot determine intent.
- SCU = Security Compute Units; each prompt/agent run consumes capacity.
- Purview Unified Audit Log can lag (platform characteristic).

## Scoring
- Any **Blocker = Fail** ⇒ NO-GO until resolved.
- Only **Risk** items failing ⇒ GO-WITH-RISKS (list mitigations).
- All pass ⇒ GO.
