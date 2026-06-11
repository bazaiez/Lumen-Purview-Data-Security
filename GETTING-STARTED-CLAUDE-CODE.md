# 🚀 Getting Started — From Zero to Productive in 5 Minutes

**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development

This guide takes you from never having used Lumen to generating your first Security Copilot prompt.

---

## 📥 Step 1: Open the Repository

### Option A: VS Code Extension (Recommended)

1. Open VS Code (version 1.98.0 or later)
2. Go to Extensions (`Ctrl+Shift+X`) and search **"Claude Code"**
3. Install the extension by Anthropic
4. Open this repo folder in VS Code
5. Open the Claude Code panel from the sidebar

### Option B: CLI

```powershell
# Windows
irm https://claude.ai/install.ps1 | iex

# macOS / Linux
curl -fsSL https://claude.ai/install.sh | bash
```

Then navigate to this repo and run:
```
claude
```

### Option C: Desktop App

1. Download from [claude.com/download](https://claude.com/download)
2. Open the app and go to the **Code** tab
3. Point it to this repo folder

### 🔑 Account Requirement

Claude Code requires a **Pro, Max, Team, or Enterprise** plan.

---

## ⚙️ Step 2: Understand What Happens Automatically

When you open this repo, Claude automatically reads `CLAUDE.md`. This loads:

- 🤖 The complete 6-agent system (Orchestrator, Knowledge, Prompt Generator, Investigation, Demo Architect, Quality Reviewer)
- 🔌 All 6 validated Purview plugin capabilities
- ⚠️ Known SC limitations
- 📐 Prompt design rules, patterns, and taxonomy
- 📂 The full repository structure

**You don't need to configure anything. Just start talking.**

---

## 💬 Step 3: Talk to the Agents

The Orchestrator Agent receives every request and routes it to the right specialist. You don't need to know which agent handles what — just describe what you need.

### 🧪 Ask a Question

```
Can Security Copilot detect when someone copies files to USB?
```
The **Knowledge Agent** answers with capabilities, limitations, prerequisites, and example prompts.

### ⚡ Generate Prompts for a Use Case

```
Generate prompts for a SOC team triaging 200 DLP alerts per day in financial services
```
The **Prompt Generator Agent** creates complete, copy-paste-ready SC prompts from scratch. No templates.

### 🔍 Investigate an Incident

```
Investigate: a departing employee downloaded 500 confidential files to personal OneDrive last week
```
The **Investigation Agent** builds a step-by-step playbook with SC prompts at every step, decision trees, remediation actions, and escalation criteria.

### 🎬 Prepare a Customer Demo

```
I have a healthcare customer meeting Friday. They want to see how SC handles PHI protection. 
Their SOC team has 5 analysts and they triage about 100 DLP alerts per day.
```
The **Demo Architect Agent** designs a complete demo package: realistic scenarios, SC prompt scripts, narration, talking points, Q&A preparation, and hands-on lab exercises.

### 🎛️ Handle a Complex Scenario

```
Financial services customer with insider risk concerns, 500 alerts/day, 3-person SOC. 
I need: triage prompts + investigation playbook + executive reporting + a half-day workshop.
```
The **Orchestrator** splits this across all agents, coordinates the work, and delivers a unified package.

### ✅ Quality Check Before Delivery

```
Review these prompts I wrote and validate them against real SC capabilities
```
The **Quality Reviewer Agent** checks every prompt against the 6 validated capabilities, flags issues, and provides corrected versions.

---

## 📖 Step 4: Use Pre-Built Content (Optional)

If you want to browse existing content instead of generating from scratch:

| Content | Location | What's There |
|---|---|---|
| 🛡️ DLP prompts | [prompts/dlp/prompt-book.md](prompts/dlp/prompt-book.md) | Alert triage, investigation, false positive analysis |
| 👤 IRM prompts | [prompts/irm/prompt-book.md](prompts/irm/prompt-book.md) | Insider risk case management |
| 📊 DSPM prompts | [prompts/dspm/prompt-book.md](prompts/dspm/prompt-book.md) | Data security posture queries |
| 🔎 Audit prompts | [prompts/audit/prompt-book.md](prompts/audit/prompt-book.md) | Timeline reconstruction, forensics |
| 🏷️ MIP prompts | [prompts/mip/prompt-book.md](prompts/mip/prompt-book.md) | Sensitivity label management |
| 📊 Executive prompts | [prompts/executive/prompt-book.md](prompts/executive/prompt-book.md) | Leadership reporting |
| 🎓 Workshop prompts | [prompts/workshop/prompt-book.md](prompts/workshop/prompt-book.md) | Customer demos and training |
| ⚙️ Operations prompts | [prompts/operations/prompt-book.md](prompts/operations/prompt-book.md) | Policy hygiene, daily ops |
| 🔬 DFIR prompts | [prompts/dfir/prompt-book.md](prompts/dfir/prompt-book.md) | Deep forensic investigation (40 prompts) |
| 📓 Investigation playbooks | [playbooks/](playbooks/) | Step-by-step incident response |
| 📋 Use case catalog | [use-cases/catalog.md](use-cases/catalog.md) | 22 mapped scenarios |

---

## 🤖 The 6 Agents

```
👤 You  →  🎛️ Orchestrator  →  Specialist Agents  →  ✅ Quality Reviewer  →  📦 Output
```

| Agent | What It Does | When to Use |
|---|---|---|
| 🎛️ **Orchestrator** | Routes requests, plans, coordinates, synthesizes | Always (default — receives all requests) |
| 🧪 **Knowledge** | Answers SC + Purview capability questions | "Can SC do X?", "What are the limitations?" |
| ⚡ **Prompt Generator** | Crafts SC prompts from scratch for any use case | "Generate prompts for..." |
| 🔍 **Investigation** | Builds incident response playbooks | "Investigate...", "What happened with..." |
| 🎬 **Demo Architect** | Designs customer demos and workshops | "Prepare a demo for...", "Workshop ideas" |
| ✅ **Quality Reviewer** | Validates all outputs against real capabilities | Before any customer delivery |

Full agent documentation: [agents/AGENT-HUB.md](agents/AGENT-HUB.md)

---

## 💡 Tips

- 🎯 **Be specific about customer context** — industry, team size, pain points, and data types produce better results
- 📓 **Ask for promptbooks** — multi-step workflows are more useful than single prompts
- ⚠️ **Request limitations** — every output should include what SC cannot do
- 🏷️ **Check validation status** — [VALIDATED] means tested and working; [ASPIRATIONAL] means untested
- 🔤 **Use exact names** — SC works better with exact SIT names ("Credit Card Number") and audit log operations (FileDownloaded) than natural language approximations

---

## 🛠️ Useful Claude Code Commands

| Command | What It Does |
|---|---|
| `/compact` | Compresses conversation context when it gets long |
| `/clear` | Starts a fresh conversation |
| `/memory` | Opens your memory file for persistent notes |
| `claude -c` | Resume the most recent conversation (CLI) |

---

## 🔗 How It All Connects

```
👤 You ──► Claude Code ──► Reads CLAUDE.md (6-agent system, capabilities, rules)
                │
                ├──► 🤖 agents/     (6 specialist agents with full system prompts)
                ├──► 📝 prompts/    (pre-built prompt books by domain)
                ├──► 🔍 playbooks/  (investigation workflows)
                ├──► 📋 use-cases/  (22 mapped customer scenarios)
                ├──► 📚 docs/       (technical deep dives, limitations, mechanics)
                └──► 📐 templates/  (structures for new content)
```

Claude uses this entire knowledge base to generate, validate, and package SC-ready outputs.

---

## ▶️ Next: Try It

Open Claude Code, point it at this repo, and type:

```
I have a retail customer worried about credit card data leaking through email. 
Their compliance team needs weekly reports for the board. Generate everything I need.
```

The agents will do the rest. 🚀
