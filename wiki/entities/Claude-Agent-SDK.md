---
type: entity
tags: [product, sdk, anthropic, claude-code, agents, developer-tools]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# Claude Agent SDK

**Type**: product
**Also known as**: Claude Code SDK (former name), the SDK

## Overview

The Claude Agent SDK is Anthropic's developer toolkit for building general-purpose AI agents on top of Claude. Originally developed as the agent harness powering Claude Code (and named the Claude Code SDK), it was renamed in September 2025 to reflect its evolution beyond coding tasks. Anthropic now uses it internally for deep research, video creation, note-taking, and "almost all major agent loops." The core design principle: give agents access to a computer (file system, bash, code execution) so they can work the way humans do.

## Key Facts

| Attribute | Value |
|-----------|-------|
| Creator | [[Anthropic]] |
| Former name | Claude Code SDK |
| Renamed | September 2025 |
| Core primitive | Computer access via bash, file system, code execution |
| Built-in features | Subagents, compaction, MCP integrations, tool framework |
| Runs on | Claude Code infrastructure |

## Core Capabilities

- **Agentic search** — file system exploration via bash/grep as primary retrieval (see [[Agentic Search]])
- **Subagents** — parallel and context-isolated specialist agents built in by default
- **Compaction** — automatic context summarization when approaching context limits (see [[Context Compaction]])
- **Custom tools** — developer-defined primary actions, prominent in the context window
- **Bash & scripts** — flexible general-purpose computer access
- **Code generation** — precise, composable, reusable programmatic outputs
- **MCP integrations** — standardized connections to Slack, GitHub, Asana, and other external services without custom OAuth

## The Agent Loop

The SDK is designed around a three-phase loop:

1. **Gather context** — agentic search, semantic search (optional), subagents for parallel information gathering
2. **Take action** — custom tools, bash scripts, code generation, MCP calls
3. **Verify work** — rules-based feedback, visual feedback (screenshots), LLM-as-Judge

## Role in This Wiki

The Claude Agent SDK is the platform on which the [[Agentic Coding Rollout]] described in [[Scaling Agentic Coding Across Your Organization]] is built. [[Brex]] and [[Rakuten]] build domain-specific agents on top of it. The [[LLM Wiki Pattern]] itself could be implemented as a Claude Agent SDK agent.

## Appears In

[[Building Agents with the Claude Agent SDK]]

## Related

- [[Anthropic]] — creator and primary user of the SDK
- [[Agentic Search]] — the recommended primary retrieval pattern
- [[Context Compaction]] — built into the SDK via automatic compact feature
- [[Multi-Agent Architecture]] — subagents supported by default
- [[Brex]] — builds domain-specific agents on the SDK
- [[Rakuten]] — builds domain-specific agents on the SDK
- [[CLAUDE.md at Scale]] — CLAUDE.md is the configuration layer that governs SDK-based agents
