---
type: concept
tags: [ai-agents, architecture, workflow, evaluation, quality, iteration]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf, https://www.anthropic.com/engineering/multi-agent-research-system, https://claude.com/blog/multi-agent-coordination-patterns]
---

# Evaluator-Optimizer Workflow

**Domain**: AI agent architecture
**Also called**: generate-evaluate-refine, critic pattern, iterative refinement, Generator-Verifier

## Definition

An evaluator-optimizer workflow pairs a generator agent with one or more evaluator agents in a feedback loop. The generator produces output; evaluators assess it against defined criteria; the generator refines based on feedback; the cycle repeats until quality thresholds are met. It is the agent architecture for tasks where first-draft quality is insufficient and independent review is required.

## How It Works

```
Task → Generator → Output → Evaluator(s) → Feedback
           ↑                                    |
           └────────── Refinement ←─────────────┘
```

Key components:
- **Generator agent** — produces the initial output and iteratively refines it
- **Technical evaluator agent** — validates against factual or technical criteria (e.g. accuracy, correctness, coverage)
- **Quality evaluator agent** — assesses against qualitative criteria (e.g. clarity, usability, tone)
- **Termination condition** — criteria that end the loop (quality threshold met, max iterations reached)

Example — API documentation generator:
1. Development team submits codebase
2. **Generator agent** — analyzes code, creates initial documentation (endpoints, parameters, examples, auth)
3. **Technical evaluator** — validates accuracy against actual code (parameter types, endpoint coverage, example correctness)
4. **Quality evaluator** — assesses developer usability (clarity, completeness, readability)
5. Generator incorporates feedback from both evaluators, iterates
6. Loop continues until both evaluators approve or max iterations are reached

**When to use:**
- High-quality output is required and first-draft generation is insufficient
- Independent validation adds meaningful signal (not just the generator checking its own work)
- Draft-review-polish workflows
- When the evaluation criteria can be formally specified

**When to avoid:**
- When quality standards don't require independent review
- When iteration cycles are too slow for the use case
- When a single skilled agent with good prompting can meet quality standards directly

**Critical limitation** (from [[Multi-Agent Coordination Patterns]]): "The verifier is only as good as its criteria." Vague or incomplete evaluation criteria undermine the entire pattern — the generator refines toward a poorly-specified target. The pattern also requires fallback strategies: if the generator cannot address the feedback (e.g. the task is genuinely infeasible), the loop stalls. Explicit maximum iteration limits and human escalation paths are required.

## End-state evaluation principle

From [[How We Built Our Multi-Agent Research System]]: focus evaluation on **final outcomes** rather than validating every intermediate step. Agents may find alternative valid paths to correct answers — evaluating intermediate steps forces a single "correct" path and misses valid alternatives. End-state evaluation is more robust to the natural non-determinism of agentic systems.

## [[LLM-as-Judge]] for scale

The same article introduces LLM-as-Judge as the production-scale instantiation of the evaluator role. Rather than a human reviewing every output, an LLM evaluator scores outputs against rubric criteria (factual accuracy, citation accuracy, completeness, source quality, tool efficiency). Start with small eval sets (~20 queries) to catch major issues before scaling. Human review remains necessary to catch biases LLM judges miss.

## Open question

How do evaluator-optimizer loops avoid infinite refinement? Termination requires either explicit iteration limits or convergence criteria — the whitepaper does not specify how to set these in practice. The Research system addresses this via end-state evaluation: the lead agent determines sufficiency rather than running a fixed number of evaluator cycles.

## Key Sources

[[Building Effective AI Agents]], [[How We Built Our Multi-Agent Research System]], [[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Multi-Agent Architecture]] — evaluator-optimizer is one of the core patterns
- [[Sequential Workflow]] — the evaluator-optimizer adds a feedback loop to what would otherwise be a sequential flow
- [[Single-Agent Architecture]] — try this first; evaluator pattern adds cost and latency
- [[LLM-as-Judge]] — production-scale instantiation of the evaluator role
- [[Self-Improving Agent]] — related but distinct: improves the system across runs, not output within a run
