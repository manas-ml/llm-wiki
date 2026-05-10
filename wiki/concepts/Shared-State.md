---
type: concept
tags: [ai-agents, architecture, multi-agent, coordination, shared-memory, persistence]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/multi-agent-coordination-patterns]
---

# Shared State

**Domain**: AI agent architecture, multi-agent coordination
**Also called**: shared memory coordination, collaborative knowledge store, agent blackboard

## Definition

Shared State is a multi-agent coordination pattern where agents operate autonomously, reading and writing to a common persistent store (database, file system, or shared document) rather than communicating through a coordinator or message broker. Work begins when initialization seeds the store; agents discover what's been done by reading it and contribute by writing to it; the system terminates when a defined condition is met. Discoveries flow immediately to all agents — no central point of failure, no routing delay.

## How It Works

```
Agent A ─┐                    ┌─ Agent B
          ↕                    ↕
Agent C ─→ [Persistent Store] ←─ Agent D
          ↕                    ↕
Agent E ─┘                    └─ Agent F
```

1. **Initialization** seeds the persistent store with the starting state (research questions, document structure, initial findings)
2. **Agents read** from the store to understand what's been found and what remains
3. **Agents write** their findings, updates, and intermediate results directly to the store
4. **All agents see updates immediately** — an agent's discovery is available to all others on next read
5. **Termination** triggers when a defined condition is met (time budget exhausted, convergence threshold reached, all questions answered)

### Strengths

- **Immediate sharing** — findings propagate to all agents without routing; Agent A's discovery is instantly available to Agent B
- **No single point of failure** — if one agent stops or fails, others continue; no coordinator to bring down the system
- **Emergent collaboration** — agents can build on each other's work organically without explicit coordination
- **Natural persistence** — the store is both the communication channel and the durable output

### Weaknesses and failure modes

- **Duplication** — without coordination, multiple agents may work on the same question or file simultaneously
- **Contradictory approaches** — agents may pursue conflicting strategies without a coordinator to resolve conflicts
- **Unconvergent loops** — reactive agents reading each other's writes can create circular update chains that never settle; requires first-class termination logic
- **Termination design** — this is the most critical challenge; systems without clear convergence conditions can run indefinitely or stop prematurely. Time budgets, convergence thresholds, and "all questions answered" checks must be explicitly designed

### Termination strategies

| Strategy | Mechanism | Best for |
|----------|-----------|---------|
| Time budget | System stops after N minutes regardless of state | Research synthesis, exploration |
| Convergence threshold | System stops when change rate drops below threshold | Knowledge building, document synthesis |
| Completion check | System stops when all defined tasks are marked done | Structured analysis with known scope |
| Human review gate | System pauses for human inspection before continuing | High-stakes outputs |

### When to use vs. alternatives

| Choose Shared State when... | Choose instead when... |
|----------------------------|----------------------|
| Agents' work is collaborative with real-time finding exchanges | Agents work on separate, independent partitions → [[Agent Teams]] |
| Discoveries from one agent should immediately influence others | Discrete event reactions → [[Message Bus]] |
| No single point of failure is required | Centralized oversight is needed → [[Hierarchical Multi-Agent System]] |
| Agents accumulate findings over time | One-shot parallel tasks → [[Parallel Workflow]] |

### Example use case

Research synthesis: multiple investigation threads explore a complex topic. Agent A researches historical context and writes findings to a shared document. Agent B, researching current developments, reads Agent A's findings and builds on them, noting connections. Agent C, doing competitive analysis, reads both and flags a contradiction. No coordinator routes this — agents discover each other's work by reading the store. The system terminates after 30 minutes (time budget) or when all defined research questions have entries (completion check).

## Relationship to This Wiki

This wiki's `wiki/` directory is a form of shared state: pages are the persistent store, and each INGEST operation reads what exists and writes new content. A multi-agent wiki ingest could have multiple agents reading `index.md` and writing new entity/concept pages simultaneously — but without coordination on `index.md` and `log.md`, writes would conflict. Shared state works for the pages themselves; the index and log require coordination (a light orchestrator layer).

## Key Sources

[[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Agent Teams]] — alternative when agents work on separate partitions without needing real-time sharing
- [[Message Bus]] — alternative for discrete event reactions rather than accumulated state
- [[Multi-Agent Architecture]] — the broader category; Shared State is one of five named coordination patterns
- [[Collaborative Multi-Agent System]] — the existing wiki concept for peer-to-peer agent coordination; Shared State is a specific implementation mechanism for it
- [[Long-Running Agent]] — shared state systems are often long-running; termination design applies doubly
- [[Durable Agent Execution]] — persistent store reliability is critical; store failures bring down the whole system
