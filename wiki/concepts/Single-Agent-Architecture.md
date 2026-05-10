---
type: concept
tags: [ai-agents, architecture, single-agent]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf]
---

# Single-Agent Architecture

**Domain**: AI agent architecture
**Also called**: single agent, standalone agent

## Definition

A single-agent architecture deploys one AI model as the reasoning engine for a task, extended with a prompt defining its role, a toolkit of integrations, and optionally [[Agent Skills]] for specialized domain knowledge. It is the recommended starting point for any agentic system — deploy in weeks, validate value, then scale if needed.

## How It Works

Core components:
1. **AI model** — reasoning engine (e.g. Claude)
2. **Prompt / schema** — defines the agent's role, constraints, and workflows
3. **Tool integrations** — APIs, databases, file systems, search
4. **Agent Skills** — modular domain knowledge packages (optional but powerful)

**When to use:**
- Open-ended problems where the path isn't predetermined
- Single domain, even with complex workflows
- When you need to prove ROI before committing to a multi-agent investment
- Tasks that benefit from a single coherent reasoning thread

**When to avoid:**
- Tasks requiring simultaneous pursuit of many independent directions
- When specialized expertise across multiple distinct domains must be applied concurrently
- When quality demands multiple independent reviews (→ use [[Evaluator-Optimizer Workflow]])

## Scaling path

Single agents scale further than most teams expect, especially with well-designed [[Agent Skills]]. The recommended progression:
1. Start with a single agent + minimal skills
2. Add skills as domain complexity grows
3. Add a second agent (evaluator) only when quality demands it
4. Expand to multi-agent only when throughput or domain breadth genuinely requires it

## Relevance to This Wiki

This wiki is a single-agent system. Claude is the agent; [[CLAUDE.md Schema]] is the prompt/skill; read/write file tools are the integrations. It handles ingest, query, and lint operations without any multi-agent coordination.

## Key Sources

[[Building Effective AI Agents]]

## Related Concepts

- [[AI Agent]] — the general concept
- [[Agent Skills]] — what makes single agents powerful before multi-agent is needed
- [[Multi-Agent Architecture]] — the next step when single agent genuinely isn't enough
- [[LLM Wiki Pattern]] — this wiki as a single-agent instance
