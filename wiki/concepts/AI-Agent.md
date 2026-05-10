---
type: concept
tags: [ai-agents, llm, automation, architecture]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf]
---

# AI Agent

**Domain**: artificial intelligence, software systems
**Also called**: autonomous agent, LLM agent, agentic AI

## Definition

An AI agent is an LLM-powered system that autonomously solves problems through open-ended, multi-step processes where the path forward is not predetermined. The key distinction from standard generative AI: "Generative AI answers questions. AI agents solve problems." Agents can plan, use tools, take actions, observe results, and iterate — without a human directing each step.

## How It Works

An agent loop typically runs:
1. **Perceive** — receive a task or observe state (from user, environment, or another agent)
2. **Plan** — use the LLM to determine what to do next
3. **Act** — call tools, APIs, databases, or other agents
4. **Observe** — receive results from actions
5. **Iterate** — repeat until the task is complete or a stopping condition is met

Core components of any agent system:
- **AI model** — the reasoning engine (e.g. Claude)
- **Prompt** — defines the agent's role, constraints, and capabilities
- **Tool integrations** — what the agent can do (search, write code, call APIs, read files)
- **[[Agent Skills]]** — modular capability packages with domain knowledge and workflows

## Agents vs. Generative AI

| Dimension | Generative AI | AI Agent |
|-----------|---------------|----------|
| Task type | Answering questions | Solving problems |
| Path | Predetermined | Open-ended |
| Steps | Single | Multi-step |
| Tools | Rarely | Central |
| Iteration | No | Yes — acts on results |
| Autonomy | Low | High |

## Relevance to This Wiki

The [[LLM Wiki Pattern]] is itself a single-agent system — Claude acts as an agent maintaining the wiki, with CLAUDE.md as its prompt, and file read/write as its tools. Understanding AI agent architecture illuminates how and why this wiki system works.

## Key Sources

[[Building Effective AI Agents]]

## Related Concepts

- [[Single-Agent Architecture]] — the simplest agent deployment
- [[Multi-Agent Architecture]] — multiple agents coordinating
- [[Agent Skills]] — modular capability extensions
- [[LLM Wiki Pattern]] — this wiki as an agent system instance
- [[CLAUDE.md Schema]] — the prompt/schema that defines this wiki's agent behavior
