---
type: concept
tags: [evaluation, ai-agents, quality-control, testing, llm]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/multi-agent-research-system, https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# LLM-as-Judge

**Domain**: AI evaluation, quality assurance, agent testing
**Also called**: LLM-based evaluation, automated rubric evaluation, model-graded evaluation

## Definition

LLM-as-Judge is an evaluation approach where a language model assesses agent outputs against a rubric of defined criteria, replacing or augmenting human evaluation at scale. Rather than manually reviewing every agent response, an evaluator model scores outputs on specific dimensions — providing structured, repeatable, scalable quality assessment that can run automatically as part of a CI/CD or testing pipeline.

## How It Works

### Evaluation rubric

The judge model receives agent output alongside a rubric specifying dimensions to evaluate. In Anthropic's Research system, the rubric included:

| Dimension | What it measures |
|-----------|-----------------|
| Factual accuracy | Are claims grounded in sources? |
| Citation accuracy | Do citations correctly attribute claims? |
| Completeness | Does the response cover the key aspects of the question? |
| Source quality | Are sources authoritative, not just SEO-optimized? |
| Tool efficiency | Did the agent use tools appropriately without waste? |

The judge scores each dimension and optionally provides reasoning. Aggregated scores enable systematic comparison across prompt versions, model configurations, or task types.

### Why it works

LLM judges can evaluate dimensions — like citation accuracy or source authority — that are tedious but tractable for models. They enable:
- **Scale**: Evaluate hundreds of outputs where human review would take days
- **Consistency**: Same rubric applied identically across all evaluations
- **Speed**: Catch major issues in small eval sets (20 queries) before committing to large-scale testing

### Starting small

Anthropic's guidance: begin evaluation with ~20 representative queries. This small set reveals dramatic impact from prompt tweaks at low cost. Only scale up after major issues are resolved.

### Honest assessment of robustness

[[Building Agents with the Claude Agent SDK]] offers a more cautious view than the Research system article: LLM-as-Judge "is generally not a very robust method, and can have heavy latency tradeoffs." The article recommends it only for cases "where any boost in performance is worth the cost." This tension is worth holding:

- **[[How We Built Our Multi-Agent Research System]]** endorses it for *pipeline-level evaluation* (assessing many outputs systematically against rubrics)
- **[[Building Agents with the Claude Agent SDK]]** cautions against it as an *in-loop verification step* for individual agent outputs

The distinction: LLM-as-Judge works best as an offline/batch evaluator for system improvement, less well as a real-time quality gate within each agent run.

### Limitations — what LLM judges miss

Human testing catches failure modes that automated evaluation misses:

- **Subtle biases** — e.g. preference for SEO-optimized content over authoritative but less prominent sources; the LLM judge may not penalize this if both sources look credible
- **Edge cases** — unusual queries that expose gaps the rubric didn't anticipate
- **Tone and usability** — qualitative dimensions that resist rubric encoding

The recommended approach is a **three-layer eval stack**:
1. Small manual eval → catch major issues early
2. LLM-as-judge → scale systematic quality measurement
3. Periodic human review → catch biases and edge cases automation misses

### Relationship to [[Evaluator-Optimizer Workflow]]

The [[Evaluator-Optimizer Workflow]] uses an evaluator agent within a task run — it assesses draft output and feeds back to a generator. LLM-as-Judge is a *testing infrastructure* pattern — it evaluates completed outputs after the fact, across many runs, to guide system improvement. One is in-loop; the other is out-of-loop.

### End-state evaluation

A related principle: focus evaluation on **final outcomes** rather than validating every intermediate step. Agents may find alternative valid paths to correct answers. Evaluating intermediate steps forces a single "correct" path and misses valid alternatives. End-state evaluation is more robust to the natural non-determinism of agentic systems.

## Relevance to This Wiki

The [[CLAUDE.md Schema]] LINT operation is a manual analog of LLM-as-Judge — Claude audits the wiki against a defined checklist (broken links, missing index entries, frontmatter gaps) and reports findings. An automated version would run LINT on a schedule and log findings without human initiation.

## Key Sources

[[How We Built Our Multi-Agent Research System]], [[Building Agents with the Claude Agent SDK]]

## Related Concepts

- [[Evaluator-Optimizer Workflow]] — in-loop evaluator; LLM-as-Judge is out-of-loop
- [[Self-Improving Agent]] — LLM-as-Judge provides the feedback signal for self-improvement
- [[Agentic ROI Measurement]] — LLM-as-Judge is a mechanism for measuring quality impact
- [[Human-in-the-Loop Oversight]] — human review complements LLM-as-Judge for edge cases and biases
- [[Test-Driven Agent Development]] — complementary: TDD defines success criteria upfront; LLM-as-Judge evaluates whether they're met
