---
type: concept
tags: [ai-agents, architecture, multi-agent, coordination, stateful, workers]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/multi-agent-coordination-patterns]
---

# Agent Teams

**Domain**: AI agent architecture, multi-agent coordination
**Also called**: persistent worker agents, stateful agent teams, agent pool

## Definition

Agent Teams is a multi-agent coordination pattern where a coordinator spawns multiple persistent worker agents that claim tasks from a shared queue, work autonomously across multiple steps, and signal completion when done. Unlike ephemeral subagents that receive a task and return a result, worker agents in this pattern are long-lived — they retain context across assignments and accumulate domain specialization over time, improving performance as the work progresses.

## How It Works

```
                ┌─────────────┐
                │  Shared     │
Coordinator →   │  Task Queue │ ← Worker A (persistent, multi-step)
                │             │ ← Worker B (persistent, multi-step)
                └─────────────┘ ← Worker C (persistent, multi-step)
```

1. **Coordinator** seeds the shared task queue with an initial set of tasks
2. **Worker agents** claim tasks independently — each worker handles its task across as many steps as needed
3. **Workers retain context** across assignments, accumulating knowledge of the domain (file structure, patterns encountered, decisions already made)
4. **Workers signal completion** and optionally claim new tasks from the queue
5. The system terminates when the queue is empty or a termination condition is reached

### Key distinction from [[Parallel Workflow]]

| Dimension | Parallel Workflow (fan-out/fan-in) | Agent Teams |
|-----------|-----------------------------------|-------------|
| Agent lifetime | Ephemeral — spawned per task, terminated on completion | Persistent — lives across multiple tasks |
| State | Stateless — fresh context each task | Stateful — accumulates context and specialization |
| Task assignment | Fixed at spawn time by orchestrator | Dynamic — workers claim from shared queue |
| Context continuity | None across tasks | Yes — prior work informs current task |
| Best for | Independent parallel tasks with defined outputs | Multi-step subtasks benefiting from accumulated context |

### Strengths

- **Context accumulation** — workers learn the domain as they work (e.g. a migration agent learns the codebase's patterns by service 3, making services 4–10 faster)
- **Dynamic load balancing** — faster workers claim more tasks naturally via the shared queue
- **Independence** — clean task partitions allow parallel execution without inter-agent coordination

### Weaknesses and failure modes

- **Completion detection** — variable task durations make it hard to know when the system is "done"; requires robust signaling
- **Resource conflicts** — concurrent workers can conflict on shared resources (same file, same database table); requires careful partition design
- **Queue design dependency** — the pattern's effectiveness depends heavily on how well tasks are partitioned in the queue; poorly partitioned tasks cause duplication or gaps

### When to use vs. alternatives

| Choose Agent Teams when... | Choose instead when... |
|---------------------------|----------------------|
| Subtasks require multi-step work with context continuity | Subtasks are short and self-contained → [[Hierarchical Multi-Agent System]] |
| Workers benefit from accumulated domain knowledge | Tasks are fully independent with no context benefit → [[Parallel Workflow]] |
| Dynamic load balancing matters | Task sequence is predetermined → [[Sequential Workflow]] |
| Large corpus requires partitioned parallel processing | Agents need to share discoveries in real-time → [[Shared State]] |

### Example use case

A framework migration project: service A to service Z must be migrated independently. Each worker agent claims a service, migrates it across many steps (read, understand patterns, rewrite, test, fix), and signals completion before claiming the next service. By service 5, each worker has learned the codebase's idiosyncrasies and works faster. Workers don't interfere because services are independent.

## Relevance to This Wiki

A multi-source batch ingest of this wiki could be organized as an Agent Teams pattern: each worker agent claims a source from a queue, runs the full INGEST workflow for that source, and updates shared state (index.md, log.md). Workers must coordinate on shared resources (index.md, log.md) — a concrete example of the resource conflict challenge requiring careful design.

## Key Sources

[[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Parallel Workflow]] — sibling pattern; ephemeral parallel agents vs. persistent stateful workers
- [[Hierarchical Multi-Agent System]] — alternative for short, bounded subtasks with a central orchestrator
- [[Shared State]] — complementary pattern when agents need to share discoveries, not just results
- [[Multi-Agent Architecture]] — the broader category; Agent Teams is one of five named coordination patterns
- [[Long-Running Agent]] — Agent Teams workers are a form of long-running agent applied at the team level
- [[Durable Agent Execution]] — production infrastructure required for persistent worker agents
