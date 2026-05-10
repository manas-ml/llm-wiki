---
type: concept
tags: [ai-agents, architecture, multi-agent, collaborative, distributed]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf]
---

# Collaborative Multi-Agent System

**Domain**: AI agent architecture
**Also called**: peer-to-peer agents, distributed agent system, swarm (loosely)

## Definition

A collaborative multi-agent system deploys autonomous agents that communicate, coordinate, and collaborate through peer interactions rather than top-down delegation. Coordination emerges from agent interactions, not from a central authority. Agents negotiate roles dynamically, share findings in real-time, and collectively solve complex problems through distributed intelligence.

## How It Works

Three implementation variants:

**Group chat orchestration** — Multiple agents solve problems through a shared conversation thread. Each agent contributes its perspective; coordination happens through discussion rather than routing.

**Event-driven coordination** — Events serve as a shared language. Agents emit and consume events, acting as structured communication units that decouple agents while enabling rich information sharing.

**Blackboard/shared memory** — Agents read from and write to a shared state store. Any agent can update the shared context; all agents observe the evolving state.

Example — competitive intelligence system:
1. Client requests comprehensive competitive analysis
2. Pricing, product, marketing, financial, social media, and strategic intelligence agents **establish communication channels and divide responsibilities autonomously**
3. Agents share findings in real-time — pricing agents alert product agents about feature-price correlations; marketing agents share campaign data with strategic agents
4. Cross-agent collaboration builds a comprehensive picture no single agent could construct

**When to use:**
- Complex problems where diverse perspectives and real-time cross-pollination of findings add value
- Tasks where the right division of labor isn't known in advance
- When agents need to collaborate rather than hand off work
- Research, analysis, or sense-making tasks requiring emergent synthesis

**When to avoid:**
- When a clear chain of responsibility is needed (→ [[Hierarchical Multi-Agent System]])
- When coordination overhead would outweigh collaboration benefits
- When agents can complete tasks independently without real-time cross-referencing

## Note on Performance

Early benchmarking cited in the whitepaper shows "swarm architecture slightly outperforms supervisor architecture across the board" because agents can collaborate directly without supervisory translation layers.

## Key Sources

[[Building Effective AI Agents]]

## Related Concepts

- [[Multi-Agent Architecture]] — the broader category
- [[Hierarchical Multi-Agent System]] — the centralized alternative
- [[Parallel Workflow]] — often used within collaborative systems for concurrent processing
