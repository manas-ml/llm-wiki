---
type: synthesis
tags: [context-engineering, ai-agents, best-practices, llm, retrieval, compaction]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, https://claude.com/blog/building-agents-with-the-claude-agent-sdk, https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work]
---

# Best Practices for Context Engineering

**Original prompt**: What are the best practices for context engineering?
**Type**: query answer

## Findings

### The One Governing Principle

Everything derives from a single rule: **find the smallest possible set of high-signal tokens that maximizes desired outcome likelihood.** This is a response to [[Context Rot]] — a hard architectural constraint where LLM recall degrades as token count grows due to transformer n² attention relationships. Every best practice is an implementation of this principle.

---

### 1. System Prompts — Write at the Right "Altitude"

- **Avoid two failure modes**: too vague (false shared context) and too brittle (hardcoded complex logic)
- **Structure with clear sections**: XML tags or Markdown headers (`<instructions>`, `## Tool guidance`, `<background>`) help the model attend to relevant parts
- **Start minimal, add based on failures**: don't anticipate every edge case upfront; test with the minimal prompt first and add instructions only after observing real failure modes
- **Use heuristics, not rules**: strong signals the agent can reason from, not exhaustive decision trees

---

### 2. Tools — Design for Token Efficiency

- Tools are prominent in the context window — they compete for attention budget
- **Unambiguous inputs and descriptions**: if you can't definitively specify when to use a tool, the agent can't do better than guessing. Overlapping tool functionality is a primary failure mode
- **Token-efficient returns**: store large results externally and return reference pointers, not full payloads inline
- Audit tool sets regularly for overlap — agents waste context on decision-making when tools overlap

---

### 3. Data Loading — Just-in-Time, Not Upfront

([[Just-in-Time Context Retrieval]], [[Agentic Search]])

- **Prefer JIT over pre-loading**: maintain lightweight identifiers and load data at runtime only when needed
- **Use [[Agentic Search]]** (file-system exploration via bash/grep) as the default — more accurate, transparent, and lower-maintenance than semantic/vector search; add vector search only when speed is a hard constraint
- **Exploit reference metadata**: file names, folder structure, and timestamps carry signal before any content is read — design information architecture so identifiers are informative
- **Hybrid strategy**: load stable context upfront (CLAUDE.md, system prompts, project config); use JIT for dynamic, task-specific content

---

### 4. History Management — Prune Continuously

([[Context Compaction]])

- **Clear raw tool outputs** once their results have been acted on — keeping them is context pollution
- **Compact proactively**: summarize and reinitialize before quality degrades, not after
- **Tune compaction prompts on real traces**: start with maximum recall, then iterate to improve precision. Overly aggressive compaction loses subtle constraints whose importance only emerges later
- The [[Claude Agent SDK]] handles compaction automatically for SDK-based agents

---

### 5. Long-Horizon Tasks — Three Complementary Techniques

([[Context Compaction]], [[Long-Running Agent]])

| Technique | Mechanism | Best for |
|-----------|-----------|---------|
| **Compaction** | Summarize + reinitialize at context limit | Tasks with extensive back-and-forth |
| **Structured note-taking** | Agent writes persistent notes outside the context window | Iterative development with clear milestones |
| **Sub-agent architectures** | Specialized agents with clean context windows return condensed summaries (1–2k tokens) | Complex parallel research and analysis |

These are complementary, not alternatives. Sub-agents are especially powerful: each sub-agent's exploration stays isolated in its own window; the orchestrator only sees the distilled summary.

---

### 6. Large Knowledge Libraries — Progressive Disclosure

([[Agent Skills]])

For agents with access to large skill or knowledge libraries:

| Tier | Size | When loaded |
|------|------|-------------|
| Metadata | ~50 tokens | Always — so the agent knows what exists |
| Full content | ~500 tokens | When the agent determines relevance |
| Deep reference | 2,000+ tokens | During active execution only |

This enables hundreds of skills or knowledge pages without context overload. The index of what exists is always present; content loads just-in-time.

---

### 7. Iterative Nature — Runtime, Not Setup

Context engineering operates at every inference step, not once at setup. Each token entering the context is a curation decision. As agents operate in loops, stale tool outputs, completed task records, and superseded reasoning should be pruned continuously — not accumulated.

---

### The Constraint to Keep Visible

Larger context windows don't solve [[Context Rot]] — they delay it. For foreseeable model sizes, context pollution and relevance concerns persist. These practices work *with* the architectural constraint rather than hoping it disappears.

## Wiki Pages Consulted

[[Context Engineering]], [[Context Rot]], [[Just-in-Time Context Retrieval]], [[Context Compaction]], [[Agentic Search]], [[Agent Skills]], [[Long-Running Agent]], [[Claude Agent SDK]]

## Underlying Raw Sources

[[Effective Context Engineering for AI Agents]], [[Building Agents with the Claude Agent SDK]], [[Building Agents with Skills: Equipping Agents for Specialized Work]]

## Confidence and Gaps

High confidence — the seven practices are consistent across all three sources, which are mutually reinforcing rather than contradictory. The main gap: these practices are described at the design level; specific thresholds (at what token count does context rot become measurable for current models, at what skill library size does metadata-tier discoverability need supplementing) are not yet in the wiki.

## Follow-up Questions

- At what token count does context rot become practically significant for Claude Sonnet/Opus in typical agent tasks?
- How do the JIT retrieval trade-offs shift as model inference speed improves — does the speed penalty of runtime I/O become negligible?
- What tooling exists for measuring context quality across agent runs (beyond manual inspection)?
