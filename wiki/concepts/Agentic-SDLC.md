---
type: concept
tags: [sdlc, ai-agents, software-engineering, coding, 2026]
created: 2026-05-10
updated: 2026-05-10
sources: [agentictrends.pdf]
---

# Agentic SDLC

**Domain**: software engineering, AI agents
**Also called**: agent-driven development, agentic software development lifecycle

## Definition

The Agentic SDLC is the transformation of the traditional software development lifecycle through AI agents handling implementation, testing, and documentation — collapsing cycle time from weeks to hours. Traditional SDLC stages (design, implement, test, document, deploy, monitor) remain, but agent-driven execution of the middle stages fundamentally changes their economics and speed.

## How It Works

Traditional SDLC vs. Agentic SDLC:

| Stage | Traditional | Agentic |
|-------|-------------|---------|
| Design / architecture | Human | Human (still critical) |
| Implementation | Human writes every line | Agent writes code from specs |
| Testing | Human writes + runs tests | Agents write, run, fix, iterate |
| Documentation | Manual, often skipped | Inline, auto-generated |
| Code review | Peer review | [[Evaluator-Optimizer Workflow]] + human spot check |
| Monitoring | Human watches dashboards | Monitoring feeds back into rapid agent iteration |
| Cycle time | Weeks | Hours to days |

### The abstraction ladder

Each era of programming reduced the gap between human thought and machine execution:
1. Machine code → Assembly
2. Assembly → C / structured programming
3. C → High-level languages (Python, Java)
4. High-level languages → Human/machine conversation (2025)
5. Conversation → **Agentic orchestration** (2026)

The agentic step doesn't replace the previous rungs — it adds a new interface layer where engineers describe what to build and agents determine how.

### What humans retain (and must retain)
- System architecture and design decisions
- Organizational context and "taste"
- Strategic problem decomposition
- Quality evaluation and approval
- High-stakes decisions flagged by agents
- Anything requiring cross-functional judgment

### Onboarding revolution
In 2025, onboarding to a new codebase compressed from weeks to hours. In 2026, organizations are expected to use this capability to dramatically accelerate team scaling and knowledge transfer.

## Relevance to This Wiki

The [[LLM Wiki Pattern]] is itself a miniature agentic SDLC for knowledge management: the "codebase" is the wiki, the "engineer" is the human curator, and Claude is the agent that implements (writes pages), tests (lint), and documents (index/log). The abstraction is the same — human provides direction, agent executes.

## Key Sources

[[2026 Agentic Coding Trends Report]]

## Related Concepts

- [[Engineer as Orchestrator]] — the human role in the agentic SDLC
- [[Long-Running Agent]] — enables the days-long implementation phases of the agentic SDLC
- [[Human-in-the-Loop Oversight]] — how quality control works in the agentic SDLC
- [[Evaluator-Optimizer Workflow]] — the agent-driven code review pattern
- [[Multi-Agent Architecture]] — the coordination layer that makes complex agentic SDLC possible
