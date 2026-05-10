---
type: concept
tags: [ai-agents, architecture, multi-agent, coordination]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf, https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, https://claude.com/blog/multi-agent-coordination-patterns]
---

# Multi-Agent Architecture

**Domain**: AI agent architecture
**Also called**: multi-agent system, agent network, agent ensemble

## Definition

A multi-agent architecture deploys multiple AI agents with specialized capabilities working toward common goals. Agents communicate directly, through shared memory, or via message queues. Internal Anthropic research shows multi-agent systems outperform single-agent systems by **90.2%** for complex tasks requiring simultaneous pursuit of multiple independent directions.

## How It Works

Multiple agents coordinate through one of several patterns:

| Pattern | Also called | Coordination style | Best for |
|---------|-------------|-------------------|---------|
| [[Hierarchical Multi-Agent System]] | Orchestrator-Subagent | Supervisor delegates to specialists | Structured tasks with clear role boundaries |
| [[Agent Teams]] | Persistent worker pool | Workers claim tasks from shared queue, retain context | Parallel workloads with multi-step subtasks |
| [[Message Bus]] | Pub/sub agent pipeline | Agents publish/subscribe via central router | Event-driven pipelines, growing agent ecosystems |
| [[Shared State]] | Agent blackboard | Agents read/write persistent store directly | Collaborative research, real-time discovery sharing |
| [[Collaborative Multi-Agent System]] | P2P coordination | Distributed, peer-to-peer | Complex problems needing diverse perspectives |
| [[Sequential Workflow]] | Pipeline | Output of one feeds next | Linear, stage-gated processes |
| [[Parallel Workflow]] | Fan-out/fan-in | Ephemeral parallel subagents | Independent tasks needing concurrent processing |
| [[Evaluator-Optimizer Workflow]] | Generator-Verifier | Generator + evaluator loop | High-quality output with explicit evaluation criteria |

**Pattern selection by situation** (from [[Multi-Agent Coordination Patterns]]):

| Situation | Recommended pattern |
|-----------|-------------------|
| Quality-critical output, explicit evaluation criteria | [[Evaluator-Optimizer Workflow]] |
| Clear task decomposition, bounded subtasks | [[Hierarchical Multi-Agent System]] |
| Parallel workload, independent long-running subtasks | [[Agent Teams]] |
| Event-driven pipeline, growing agent ecosystem | [[Message Bus]] |
| Collaborative research, agents share discoveries | [[Shared State]] |
| No single point of failure required | [[Shared State]] |

**When to use multi-agent:**
- Tasks requiring simultaneous pursuit of many independent directions
- Multiple distinct domains requiring coordination (e.g. legal + financial + technical)
- High-volume parallel processing where speed matters
- Quality requirements demanding independent review and validation
- Long-horizon tasks that exceed a single agent's context window

**When to avoid:**
- When a [[Single-Agent Architecture]] with [[Agent Skills]] can handle the task — it usually can go further than expected
- When you need results in weeks, not months (multi-agent systems take months to get right)
- When added coordination complexity doesn't deliver proportional value

### Context isolation as a design benefit

[[Effective Context Engineering for AI Agents]] adds a context-level rationale for sub-agent architectures: each sub-agent handles a focused task in a clean context window, returning a condensed summary (typically 1,000–2,000 tokens) to the orchestrator. This achieves **separation of concerns at the context level** — detailed exploration context stays isolated in sub-agents, while the orchestrator maintains a lean context focused on synthesis and coordination. This is the multi-agent pattern as [[Context Engineering]] strategy.

## Decision framework (from the whitepaper)

1. **How complex is the task?** → Simple/moderate = single agent; highly complex multi-domain = multi-agent
2. **What's the time horizon?** → Rapid prototype = single agent; long-term strategic = design for multi-agent evolution
3. **Do you need deep domain expertise?** → One domain = single agent + skills; multiple distinct domains = multi-agent
4. **What quality level is required?** → Standard = single agent; exceptional with independent validation = evaluator pattern

## Key Sources

[[Building Effective AI Agents]], [[Effective Context Engineering for AI Agents]], [[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Single-Agent Architecture]] — always try this first
- [[Hierarchical Multi-Agent System]] — most common enterprise pattern (Orchestrator-Subagent)
- [[Agent Teams]] — persistent stateful workers; better than fan-out when context continuity matters
- [[Message Bus]] — event-driven pub/sub for growing, variable-workflow systems
- [[Shared State]] — agents read/write shared store directly; for collaborative discovery
- [[Collaborative Multi-Agent System]] — distributed autonomy variant
- [[Sequential Workflow]] — linear pipeline pattern
- [[Parallel Workflow]] — ephemeral concurrent processing pattern
- [[Evaluator-Optimizer Workflow]] — quality refinement pattern (Generator-Verifier)
- [[AI Agent]] — the base unit
- [[Context Engineering]] — sub-agent architectures achieve context isolation by design
- [[Context Compaction]] — alternative technique for long-horizon context management
