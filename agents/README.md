# 🤖 Agent System — Lumen

## 🎯 Start Here

**Read [AGENT-HUB.md](AGENT-HUB.md)** — it tells you everything you need in one page.

Or just describe what you need in plain language. The Orchestrator handles the rest.

---

## ⚒️ The 6 Agents

```
👤 You  →  🎛️ Orchestrator  →  Specialist Agents  →  ✅ Quality Reviewer  →  📦 Output
```

| Agent | File | What It Does |
|---|---|---|
| 🎛️ **Orchestrator** | [orchestrator-agent.md](orchestrator-agent.md) | Receives ANY request, plans the approach, delegates to specialists, synthesizes final output |
| 🧪 **Knowledge** | [knowledge-agent.md](knowledge-agent.md) | Answers questions about SC + Purview capabilities and limitations |
| ⚡ **Prompt Generator** | [prompt-generator-agent.md](prompt-generator-agent.md) | Generates production-ready SC prompts from scratch for any use case |
| 🔍 **Investigation** | [investigation-agent.md](investigation-agent.md) | Builds incident response playbooks with SC prompts, decision trees, remediation |
| 🎬 **Demo Architect** | [demo-architect-agent.md](demo-architect-agent.md) | Designs customer demos, workshops, scenarios, talking points, hands-on labs |
| ✅ **Quality Reviewer** | [quality-reviewer-agent.md](quality-reviewer-agent.md) | Validates all outputs against real SC capabilities before delivery |

---

## 🗺️ What You Can Do

| I need to... | Agent(s) involved |
|---|---|
| ❓ Answer a customer question about SC | 🧪 Knowledge |
| ⚡ Generate prompts for a use case | ⚡ Prompt Generator |
| 🔍 Investigate a security incident | 🔍 Investigation |
| 🎬 Prepare a customer demo or workshop | 🎬 Demo Architect + ⚡ Prompt Generator |
| 🎛️ Handle a complex multi-part request | 🎛️ Orchestrator + all specialists |
| ✅ Validate prompts before customer delivery | ✅ Quality Reviewer |

---

## 💬 Quick Examples

**Question:**
> "Can Security Copilot detect when someone copies files to USB?"

**Use case → Prompts:**
> "Generate prompts for a healthcare SOC team triaging PHI-related DLP alerts"

**Incident:**
> "Investigate: departing employee downloaded 500 files to personal OneDrive"

**Demo prep:**
> "I have a financial services CISO meeting Thursday — need to demo SC for insider risk"

**Complex scenario:**
> "Retail customer, 1000 DLP alerts/day, 5-person SOC, CCPA compliance concerns. Need: triage prompts + investigation playbook + quarterly reporting template + half-day workshop"

---

## 🏗️ Architecture

Each agent has a **self-contained system prompt** that encodes its entire knowledge base. Agents can be deployed as:

- **Claude Code instances** — each agent is a Claude Code session with its system prompt
- **Claude Agent SDK processes** — each agent is an independent microservice
- **Claude Desktop projects** — each agent is a Project with its system prompt as instructions

See [architecture.md](architecture.md) for the detailed design document, topologies, and implementation guide.

---

## 📁 Other Files

| File | Purpose |
|---|---|
| [AGENT-HUB.md](AGENT-HUB.md) | Simple entry point — start here |
| [generate-prompt.md](generate-prompt.md) | Usage guide for the /generate-prompt skill |
| [architecture.md](architecture.md) | Detailed multi-agent architecture design |
| [examples/](examples/) | Scenario walkthroughs showing agent interactions |
| [topologies/](topologies/) | Predefined agent deployment patterns |

---

## 🏅 Key Principles

- ✅ **No hallucinated features** — every prompt maps to real SC capabilities
- ⚠️ **Honest about limitations** — every output includes what SC cannot do
- 👤 **Human in the loop** — agents recommend, humans decide
- 🔍 **Quality gated** — nothing reaches a customer without validation
- 🎯 **Industry-tailored** — demos and prompts are customized, not generic

---

**Version:** 2.0 | **Last Updated:** April 2026
**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development
**Status:** Production Ready — 6 agents fully implemented
