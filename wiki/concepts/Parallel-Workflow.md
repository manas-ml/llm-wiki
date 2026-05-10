---
type: concept
tags: [ai-agents, architecture, workflow, parallel, fan-out]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf, https://www.anthropic.com/engineering/multi-agent-research-system, https://claude.com/blog/multi-agent-coordination-patterns]
---

# Parallel Workflow

**Domain**: AI agent architecture
**Also called**: fan-out/fan-in, concurrent orchestration, parallel processing

## Definition

A parallel workflow distributes independent tasks across multiple agents simultaneously, with results merged or processed afterward. It mirrors the fan-out/fan-in cloud design pattern. Best when tasks require diverse perspectives or specializations and can be processed concurrently without coordination during execution.

## How It Works

```
         ┌→ Agent A →┐
Input → Orchestrator → Agent B → Aggregator → Output
         └→ Agent C →┘
```

Two sub-variants:

**Concurrent orchestration** — Multiple agents work on the same task simultaneously, each providing independent analysis from a unique perspective or specialization. Results are aggregated (fan-in). This addresses scenarios requiring diverse insights — all agents work in parallel, reducing runtime and covering the full problem space.

**Distributed specialization** — Different agents handle different subtasks simultaneously. An orchestrator divides the work, agents process their portions concurrently, results are merged.

Example — financial risk assessment:
1. Loan application submitted
2. Data aggregation agent collects all relevant data
3. **Three parallel agents run simultaneously:**
   - Credit risk agent → creditworthiness, debt ratios, payment history
   - Market risk agent → market volatility, interest rates, sector exposure
   - Operational risk agent → process risks, regulatory compliance
4. Risk synthesis agent merges all three assessments into a final report

**Measured impact:** In Anthropic's production Research system, running subagents in parallel combined with parallel tool calls within each subagent reduced research time by up to **90%** (from [[How We Built Our Multi-Agent Research System]]).

**When to use:**
- Tasks with independent subtasks that don't need real-time cross-referencing
- When speed matters and tasks can be parallelized
- When diverse perspectives on the same problem add value
- Large-scale data processing across multiple sources simultaneously

**When to avoid:**
- When agents need to coordinate in real-time as they work (→ [[Collaborative Multi-Agent System]])
- When results from one agent are needed before another can start (→ [[Sequential Workflow]])
- When a single agent can handle the task efficiently
- When subtasks require multi-step context continuity across assignments → [[Agent Teams]] is better suited

**Distinction from [[Agent Teams]]** (from [[Multi-Agent Coordination Patterns]]): Parallel Workflow uses *ephemeral* agents — spawned per task, terminated on completion, stateless. Agent Teams uses *persistent* workers that retain context across assignments. If the parallel agents benefit from remembering what they've done across multiple tasks, use Agent Teams.

## Key Sources

[[Building Effective AI Agents]], [[How We Built Our Multi-Agent Research System]], [[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Multi-Agent Architecture]] — parallel is one of the core patterns
- [[Sequential Workflow]] — the linear alternative for dependent stages
- [[Hierarchical Multi-Agent System]] — hierarchical systems often use parallel fan-out internally
- [[Collaborative Multi-Agent System]] — agents collaborate in real-time vs. parallel independent work
