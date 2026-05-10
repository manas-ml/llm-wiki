---
type: source
tags: [ai-agents, multi-agent, coordination, patterns, architecture]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/multi-agent-coordination-patterns]
---

# Multi-Agent Coordination Patterns

**File**: `https://claude.com/blog/multi-agent-coordination-patterns`
**Type**: article
**Author(s)**: Anthropic
**Date**: April 10, 2026
**Domain**: AI agent architecture, multi-agent coordination

## Summary

A taxonomy of five multi-agent coordination patterns with mechanics, limitations, use cases, and decision guidance for choosing between them. The five patterns are Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, and Shared State. The article's central argument: start with the simplest pattern that works and evolve based on observed limitations, rather than selecting based on sophistication. The right pattern depends on how agents manage context boundaries and information flow, not on the pattern's complexity.

## Key Claims

- Five distinct coordination patterns cover the space of multi-agent systems — each with specific failure modes that should guide selection
- **Generator-Verifier** (= [[Evaluator-Optimizer Workflow]]): "the verifier is only as good as its criteria" — vague evaluation standards undermine the pattern; requires fallback strategies when feedback loops stall
- **Orchestrator-Subagent** (= [[Hierarchical Multi-Agent System]]): the orchestrator becomes an information bottleneck when subagents discover cross-relevant findings; critical details are often lost through handoffs
- **[[Agent Teams]]**: persistent stateful worker agents claim tasks from a shared queue and accumulate domain specialization — distinct from ephemeral parallel subagents; best when subtask context continuity matters
- **[[Message Bus]]**: pub/sub event-driven coordination; enables variable workflows and easy capability addition; risk of silent failures when events are misclassified or dropped
- **[[Shared State]]**: agents read/write to persistent storage with no coordinator; discoveries flow immediately; risk of duplicate work, contradictory approaches, and unconvergent reactive loops without explicit termination logic
- Combining patterns is normal in production — orchestrator-subagent + shared state for collaboration-heavy subtasks; message bus + agent teams for event-driven pipelines with long-running workers
- The primary selection criterion is "how agents manage context boundaries and information flow," not pattern sophistication

## Key Entities

[[Anthropic]]

## Key Concepts

[[Agent Teams]], [[Message Bus]], [[Shared State]], [[Multi-Agent Architecture]], [[Evaluator-Optimizer Workflow]], [[Hierarchical Multi-Agent System]], [[Parallel Workflow]]

## Notable Quotes

> "The verifier is only as good as its criteria."

> "The right pattern depends on how agents manage context boundaries and information flow."

> "Start simple: begin with orchestrator-subagent, which handles the widest range of problems with least coordination overhead."

> "Production systems often blend patterns."

## Contradicts or Challenges

None identified. Extends [[Building Effective AI Agents]] (which introduced Hierarchical, Collaborative, Sequential, Parallel, Evaluator-Optimizer) by naming Agent Teams, Message Bus, and Shared State as distinct patterns. Consistent with the "start simple" guidance in [[Scaling Agentic Coding Across Your Organization]] and the orchestrator-worker production detail in [[How We Built Our Multi-Agent Research System]].

## Questions Raised

- How should teams detect that they've hit the ceiling of a simpler pattern and need to evolve — what are the observable signals?
- How does the Message Bus pattern integrate with [[Dual-Use AI Risk]] concerns — event-driven pipelines with silent failure modes seem especially hard to audit?
- What's the recommended approach for managing shared state conflicts (concurrent writes, contradictory updates) in practice?
