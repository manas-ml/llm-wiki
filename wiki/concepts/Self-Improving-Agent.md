---
type: concept
tags: [ai-agents, self-improvement, meta-learning, prompt-engineering, evaluation]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/multi-agent-research-system]
---

# Self-Improving Agent

**Domain**: AI agent architecture, meta-level agent design
**Also called**: agent self-improvement, auto-prompting, self-diagnosing agent

## Definition

A self-improving agent is an AI agent that diagnoses its own failures and generates improvements to its own prompts, tools, or workflows — reducing the burden of manual prompt engineering and accelerating system optimization. Rather than requiring humans to identify failure modes and rewrite instructions, the agent inspects its behavior, identifies what went wrong, and proposes better configurations for future runs.

## How It Works

### The mechanism

1. An agent (or designated meta-agent) is given access to its own traces — tool call logs, outputs, failure records
2. It is prompted to diagnose failure modes: where did the task go wrong, what strategies were ineffective, what information was missing?
3. It generates concrete prompt improvements: revised instructions, better tool descriptions, updated heuristics
4. The improvements are evaluated (via [[LLM-as-Judge]] or human review) and applied to the system

### Production example (Anthropic Research system)

A dedicated **tool-testing agent** was built whose sole task was evaluating tool quality and suggesting improvements. This agent:
- Tested each tool systematically against representative tasks
- Identified where tool descriptions led agents astray
- Generated revised tool descriptions and input parameter guidance
- The resulting improvements achieved a **40% reduction in task completion time** for future agents

This represents the self-improvement loop applied specifically to tool definitions rather than top-level prompts.

### Relationship to [[Evaluator-Optimizer Workflow]]

The [[Evaluator-Optimizer Workflow]] improves the *current output* — a generator refines its answer based on evaluator feedback. Self-improving agents improve the *system itself* — the prompts, tools, and instructions that govern future runs. The target of improvement differs:

| Pattern | What improves | When it applies |
|---------|--------------|-----------------|
| Evaluator-Optimizer | Current output | Within a single task run |
| Self-Improving Agent | System configuration | Across future task runs |

### Extended thinking as enabler

The article notes that extended thinking (visible reasoning) improves both planning and evaluation phases. Self-improvement tasks benefit especially from extended thinking — diagnosing failure modes requires multi-step reasoning about what the agent "intended" vs. what it "did" and why those diverged.

### Scope of improvement

Self-improvement can target multiple levels of the system:
- **Top-level prompts** — system instructions, persona, task framing
- **Tool descriptions** — what each tool is for, when to use it, expected input formats
- **Heuristics** — when to search broadly vs. narrowly, when to stop vs. continue
- **Subagent task specifications** — how the lead agent frames work for subagents

### Limits and risks

Self-improvement is powerful but requires guardrails:
- Agent-suggested improvements should be reviewed before deployment (human-in-the-loop on the improvement loop itself)
- Improvements optimized on one task distribution may degrade performance on others
- The feedback signal must be reliable — if evaluation criteria are flawed, improvements compound the flaw

## Relevance to This Wiki

The [[CLAUDE.md Schema]] is manually maintained — humans update it when conventions need to change. A self-improving approach would have Claude flag in the log when an operation step produced unexpected results, then propose schema updates. The schema's note "After each ingest or query session, if something didn't work as expected, the schema is the first thing to update" is a manual version of this loop.

## Key Sources

[[How We Built Our Multi-Agent Research System]]

## Related Concepts

- [[Evaluator-Optimizer Workflow]] — sibling pattern; improves outputs rather than system configuration
- [[LLM-as-Judge]] — the evaluation mechanism often used to validate self-generated improvements
- [[Hierarchical Multi-Agent System]] — self-improvement typically requires a dedicated meta-agent in a hierarchy
- [[Human-in-the-Loop Oversight]] — improvements should be reviewed before deployment
- [[CLAUDE.md Schema]] — a manually-maintained analog of the system configuration that self-improving agents would update automatically
