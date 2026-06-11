# Documentation

Supporting documentation for the Lumen framework.

## Start Here

If you're new to this framework, start with [GETTING-STARTED-CLAUDE-CODE.md](../GETTING-STARTED-CLAUDE-CODE.md) for the quickest path to productivity. To use the 6-agent system directly, see [agents/AGENT-HUB.md](../agents/AGENT-HUB.md).

## Contents

| Document | Purpose |
|----------|---------|
| [vision-and-scope.md](vision-and-scope.md) | What we're building and why — strategic context |
| [personas.md](personas.md) | Detailed role definitions and workflows for target users |
| [knowledge-architecture.md](knowledge-architecture.md) | How knowledge flows through Security Copilot |
| [plugin-dependency-map.md](plugin-dependency-map.md) | How prompts depend on specific plugin capabilities |
| [technical-deep-dives/](technical-deep-dives/) | In-depth technical references (see below) |
| [reference/](reference/) | Exact names for audit operations and SITs |

## Technical Deep Dives

| Document | Purpose |
|----------|---------|
| [Purview.md](technical-deep-dives/Purview.md) | Purview data flows and signal generation |
| [security-copilot-mechanics.md](technical-deep-dives/security-copilot-mechanics.md) | SC context, plugins, sessions, SCUs, and capabilities |
| [limitations-and-blind-spots.md](technical-deep-dives/limitations-and-blind-spots.md) | Honest assessment of gaps and constraints |

## Reference Data

| Document | Purpose |
|----------|---------|
| [audit-log-operations.md](reference/audit-log-operations.md) | Exact Unified Audit Log operation names for SC prompts |
| [sensitive-information-types.md](reference/sensitive-information-types.md) | Exact SIT names for SC prompts |

These reference documents are used by the [Prompt Generator Agent](../agents/prompt-generator-agent.md) and [Investigation Agent](../agents/investigation-agent.md) to ensure prompts use exact technical names instead of natural language approximations.

## Agent System Documentation

The 6-agent system that powers this framework is documented in [agents/](../agents/):

| Agent | Purpose |
|-------|---------|
| [Orchestrator](../agents/orchestrator-agent.md) | Routes requests, plans, coordinates |
| [Knowledge](../agents/knowledge-agent.md) | Answers SC + Purview capability questions |
| [Prompt Generator](../agents/prompt-generator-agent.md) | Generates SC prompts from scratch |
| [Investigation](../agents/investigation-agent.md) | Builds incident response playbooks |
| [Demo Architect](../agents/demo-architect-agent.md) | Designs customer demos and workshops |
| [Quality Reviewer](../agents/quality-reviewer-agent.md) | Validates outputs against real capabilities |

Additional reference materials (PDFs, HTML, DOCX) are available in `technical-deep-dives/`.
