---
type: concept
tags: [ai-agents, architecture, workflow, pipeline, sequential]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf]
---

# Sequential Workflow

**Domain**: AI agent architecture
**Also called**: pipeline workflow, chain workflow, stage-gated processing

## Definition

A sequential workflow passes tasks through a defined chain of agents, where each agent's output becomes the next agent's input. It is the agent equivalent of a Unix pipeline — structured, linear, stage-gated. Best for processes with clear phases where output quality must be validated before proceeding.

## How It Works

```
Input → Agent A → Agent B → Agent C → Output
```

Each stage:
- Receives output from the previous stage
- Performs its specialized transformation
- Passes the result to the next stage

Examples where sequential workflows excel:
- Generate marketing copy → translate into multiple languages
- Write document outline → validate outline meets criteria → write full document from approved outline
- Collect raw data → clean and normalize → analyze → generate report
- Code generation → testing → documentation

**When to use:**
- Linear processes with clear, ordered stages
- When output of each stage must be validated before the next begins (draft-review-polish)
- When you understand the performance characteristics of every agent in the pipeline
- When failures or delays in one stage are tolerable for overall completion

**When to avoid:**
- When agents need to collaborate rather than hand off (→ [[Collaborative Multi-Agent System]])
- When only a few stages are needed that a single agent can handle
- When the workflow requires backtracking or iteration (→ [[Evaluator-Optimizer Workflow]])
- When stages can run independently (→ [[Parallel Workflow]])

## Relationship to Unix Pipelines

Sequential agent workflows are the LLM equivalent of Unix pipelines — each stage does one thing well, output of one is input of the next, the composition produces results no single stage could. The difference: agent stages use language and reasoning rather than text stream transformations.

## Key Sources

[[Building Effective AI Agents]]

## Related Concepts

- [[Multi-Agent Architecture]] — sequential is one of the core multi-agent patterns
- [[Parallel Workflow]] — the concurrent alternative when stages are independent
- [[Evaluator-Optimizer Workflow]] — adds a feedback loop to sequential processing
