---
type: concept
tags: [ai-agents, long-running, autonomous, state-management, 2026]
created: 2026-05-10
updated: 2026-05-10
sources: [agentictrends.pdf, https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents]
---

# Long-Running Agent

**Domain**: AI agent architecture
**Also called**: persistent agent, autonomous agent, extended-horizon agent

## Definition

A long-running agent operates autonomously for extended periods — hours, days, or weeks — building complete systems with minimal human intervention. Unlike one-shot agents that handle discrete tasks in minutes, long-running agents maintain coherent state across many work sessions, plan and iterate across dozens of steps, adapt to discoveries, and recover from failures.

## How It Works

### Horizon evolution

| Era | Task horizon | Scope |
|-----|-------------|-------|
| 2024 | Minutes | Single function, single bug fix |
| 2025 | Hours | Full feature set, test suite |
| 2026 (predicted) | Days to weeks | Complete applications and systems |

### Key capabilities required
- **State management** — maintaining coherent context across sessions without "forgetting" prior decisions
- **Planning and replanning** — adapting when discoveries or failures require changing the approach
- **Failure recovery** — identifying when something went wrong and correcting course without human intervention
- **Checkpoint design** — knowing when to surface a decision to a human vs. proceeding autonomously
- **Progress tracking** — maintaining a coherent picture of what's done, what's in flight, and what's next

### Human interaction model
Long-running agents shift from continuous supervision to **periodic checkpoints**. Humans provide strategic oversight at key decision points rather than reviewing every step. The agent flags situations requiring human judgment (ambiguity, high-stakes decisions, discovered blockers) while proceeding autonomously on well-defined work.

### Infrastructure requirements (open questions from source)
- Version control workflows that handle simultaneous agent-generated contributions
- Development environments showing status of multiple concurrent agent sessions
- State persistence across context window resets
- Audit trails for agent decisions made autonomously

### Context management techniques for long-horizon tasks

Three techniques address context window limits on extended tasks (from [[Effective Context Engineering for AI Agents]]):

| Technique | Mechanism | Best for |
|-----------|-----------|---------|
| [[Context Compaction]] | Summarize conversation near limit, reinitialize with summary | Tasks with extensive back-and-forth |
| Structured note-taking | Agent writes persistent notes (NOTES.md, todo files) outside the context window | Iterative development with clear milestones |
| Sub-agent architecture | Specialized agents with clean context windows return condensed summaries (1k–2k tokens) | Complex parallel research and analysis |

[[Context Rot]] is the underlying constraint all three address: transformer n² attention means recall degrades as context grows — these techniques prevent accumulation rather than fighting it.

## Relationship to Existing Wiki Concepts

Long-running agents are the temporal extension of [[Multi-Agent Architecture]] — the same coordination patterns ([[Hierarchical Multi-Agent System]], [[Parallel Workflow]]) but operating over days rather than minutes. [[Human-in-the-Loop Oversight]] becomes the design challenge: how to structure checkpoints so human attention lands where it matters most.

## Key Sources

[[2026 Agentic Coding Trends Report]], [[Effective Context Engineering for AI Agents]]

## Related Concepts

- [[AI Agent]] — the base unit; long-running is a temporal extension
- [[Human-in-the-Loop Oversight]] — the oversight model that makes long-running agents safe
- [[Agentic SDLC]] — long-running agents enable days-long implementation phases
- [[Multi-Agent Architecture]] — long-running systems typically use multi-agent coordination
- [[Engineer as Orchestrator]] — the human role when agents run for days
- [[Context Engineering]] — the discipline that manages context health across long horizons
- [[Context Compaction]] — primary technique for surviving context window limits
- [[Context Rot]] — the architectural constraint long-running agents must design against
