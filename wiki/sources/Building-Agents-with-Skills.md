---
type: source
tags: [ai-agents, skills, domain-expertise, context-engineering, architecture, mcp]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work]
---

# Building Agents with Skills: Equipping Agents for Specialized Work

**File**: `https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work`
**Type**: article
**Author(s)**: Barry Zhang, Mahesh Murag, Keith Lazuka, Ryan Whitehead (Anthropic)
**Date**: January 22, 2026
**Domain**: AI agent architecture, domain expertise packaging, developer tooling

## Summary

An Anthropic article introducing Agent Skills as the solution to the gap between general agent capability and domain expertise. The central analogy: a math genius can solve tax problems from first principles, but an experienced tax professional who has filed thousands of returns is more useful. Skills are organized file collections (SKILL.md + docs + scripts) that package that accumulated expertise in a format agents can access progressively. The article also introduces a complete four-layer agent architecture and announces Skills as an open standard.

## Key Claims

- General capability ≠ domain expertise — agents have reasoning ability but lack accumulated professional knowledge; [[Agent Skills]] bridge this gap
- Skills are file collections, not APIs — SKILL.md plus supporting docs and executable scripts, version-controlled via Git, shareable across teams; non-engineers (PMs, analysts) can create them in ~30 minutes
- **Three-tier progressive disclosure** protects context windows: metadata tier (~50 tokens, YAML frontmatter shown at runtime), full SKILL.md (~500 tokens, loaded on skill invocation), reference docs (2,000+ tokens, loaded on demand) — enables hundreds of skills without context overload
- Skills can package executable scripts as tools — code is self-documenting, modifiable by agents, and doesn't require constant context presence; better than traditional tool definitions for most purposes
- **Complete four-layer architecture**: Agent Loop (reasoning) + Agent Runtime (code/filesystem execution) + MCP Servers (external connections) + Skills Library (domain expertise) — each layer has a distinct purpose and evolves independently
- Three skill ecosystem types: Foundational (core capabilities everyone needs), Partner (K-Dense, Browserbase, Notion making services agent-accessible), Enterprise (proprietary internal processes and compliance)
- Skills complexity scales from ~100 lines (status report writer) to 2,500+ lines (RNA sequencing pipeline coordinating HISAT2, StringTie, DESeq2)
- Skills announced as an open standard (like MCP) enabling portability across AI platforms — the same skill should function regardless of which AI platform runs it
- MCP and Skills are complementary, not competing — Skills coordinate MCP connections alongside docs and scripts in a single unified package

## Key Entities

[[Anthropic]], [[Claude Agent SDK]]

## Key Concepts

[[Agent Skills]], [[Context Engineering]], [[Just-in-Time Context Retrieval]], [[CLAUDE.md Schema]], [[Multi-Agent Architecture]], [[Single-Agent Architecture]]

## Notable Quotes

> "Agents have intelligence and capabilities, but not always the expertise to effectively tackle real work."

> "Code is not just what agents produce—it's how they work."

> "A single skill can transform an agent's capabilities. Adding the frontend design skill instantly activates Claude's frontend capabilities."

> "When someone starts using an AI agent, it should already know what the user and team care about."

## Contradicts or Challenges

No contradictions. Substantially extends [[Building Effective AI Agents]] on Agent Skills — the whitepaper introduced the concept in ~1 paragraph; this article is entirely dedicated to it. The four-layer architecture (Loop + Runtime + MCP + Skills) is the most complete architectural synthesis across any source in this wiki. The open standard announcement for Skills parallels the earlier open standard status of MCP.

## Questions Raised

- How do teams manage skill versioning when multiple enterprise teams contribute and evolve skills simultaneously?
- What are the security implications of skills that include executable scripts — how should enterprise teams audit them?
- As skills become an open standard, how do you evaluate skill quality before adding them from external sources?
- At what skill library size does the metadata-tier discoverability mechanism need to be supplemented (e.g., with semantic search over skill descriptions)?
