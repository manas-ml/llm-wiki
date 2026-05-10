---
type: source
tags: [ai-agents, sdk, claude-code, agent-loop, agentic-search, mcp]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# Building Agents with the Claude Agent SDK

**File**: `https://claude.com/blog/building-agents-with-the-claude-agent-sdk`
**Type**: article
**Author(s)**: Thariq Shihipar (with Molly Vorwerck, Suzanne Wang, Alex Isken, Cat Wu, Keir Bradwell, Alexander Bricken, Ashwin Bhat)
**Date**: September 29, 2025
**Domain**: AI agent architecture, developer tooling, SDK design

## Summary

A product and best-practices article announcing the renaming of the Claude Code SDK to the [[Claude Agent SDK]], reflecting its evolution from a coding tool to a general-purpose agent platform. The article walks through building an email agent as the running example, organized around a three-phase agent loop: gather context → take action → verify work. Introduces agentic search as the preferred retrieval mechanism over semantic/vector search, articulates a three-tier action model (tools, bash, code generation), and gives honest assessments of verification approaches including their limitations.

## Key Claims

- The Claude Code SDK is renamed to the [[Claude Agent SDK]] — Anthropic now uses it internally for deep research, video creation, note-taking, and "almost all major agent loops," not just coding
- The fundamental agent loop is: **gather context → take action → verify work → repeat** — a design pattern applicable to any agent built on the SDK
- [[Agentic Search]] (file system exploration via bash/grep) is the recommended primary retrieval approach; semantic/vector search should only be added when speed is needed — semantic search is "less accurate, more difficult to maintain, and less transparent"
- The folder and file structure of an agent is itself "a form of context engineering" — naming conventions, hierarchy, and timestamps provide signals that help agents understand and navigate information
- Three-tier action model: (1) custom tools as primary actions (prominent in context window), (2) bash scripts for flexible general-purpose work, (3) code generation for precise, composable, reusable operations
- [[MCP]] (Model Context Protocol) provides standardized external service integrations without custom OAuth or API code — growing ecosystem enables rapid capability addition
- Three verification approaches: rules-based feedback (linting, validation), visual feedback (screenshots for UI tasks via Playwright MCP), [[LLM-as-Judge]] — the article is honest that LLM-as-Judge "is generally not a very robust method" with latency tradeoffs
- [[Context Compaction]] is now built into the SDK via an automatic compact feature that triggers when the context limit approaches

## Key Entities

[[Anthropic]], [[Claude Agent SDK]]

## Key Concepts

[[Claude Agent SDK]], [[Agentic Search]], [[Just-in-Time Context Retrieval]], [[Context Compaction]], [[LLM-as-Judge]], [[Multi-Agent Architecture]], [[Context Engineering]]

## Notable Quotes

> "The key design principle behind the Claude Agent SDK is to give your agents a computer, allowing them to work like humans do."

> "The folder and file structure of an agent becomes a form of context engineering."

> "Semantic search is usually faster than agentic search, but less accurate, more difficult to maintain, and less transparent."

> "Code is precise, composable, and infinitely reusable, making it an ideal output for agents that need to perform complex operations reliably."

> "LLM as a Judge is generally not a very robust method, and can have heavy latency tradeoffs."

## Contradicts or Challenges

**Partial tension with [[How We Built Our Multi-Agent Research System]] on LLM-as-Judge**: the Research system article endorsed LLM-as-Judge as effective for automated evaluation; this article calls it "generally not a very robust method." The difference may be context: the Research system uses it at the *pipeline evaluation* level (assessing many outputs systematically), while this article cautions against using it as a *per-output verification step* within the agent loop itself. The underlying tension is real and worth tracking.

**Agentic search preference challenges RAG assumptions in [[RAG vs Wiki]]**: the article explicitly advises starting with file-system exploration over vector/embedding search, extending the wiki's existing RAG skepticism into agent design generally.

## Questions Raised

- How does the SDK's built-in compact feature compare to manually tuned compaction prompts described in [[Effective Context Engineering for AI Agents]]?
- At what scale does the "start with agentic search" guidance break down, and how should teams detect when to add semantic search?
- How does MCP server security interact with the [[Dual-Use AI Risk]] concerns — the SDK article doesn't address sandboxing, while the enterprise rollout guide does?
