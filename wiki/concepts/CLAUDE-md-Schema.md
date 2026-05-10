---
type: concept
tags: [llm, schema, configuration, claude, wiki]
created: 2026-05-10
updated: 2026-05-10

sources: [wiki.md, scalingagents.pdf]
---

# CLAUDE.md Schema

**Domain**: LLM tooling, knowledge management
**Also called**: schema document, wiki schema, AGENTS.md (for Codex)

## Definition

The CLAUDE.md schema is a configuration document that tells an LLM how a wiki is structured, what conventions to follow, and what workflows to execute when ingesting sources, answering queries, or linting the wiki. It is the key file that transforms the LLM from a generic chatbot into a disciplined wiki maintainer. For Claude Code, this file is named `CLAUDE.md`; for OpenAI Codex, it would be `AGENTS.md`.

## How It Works

The schema is read at the start of every session (as part of the Session Start Protocol) and governs all LLM behavior in the wiki. A well-designed schema contains:

- **Directory map** — what each directory is for, who owns it, access rules
- **Conventions** — wikilink syntax, frontmatter fields, filename format
- **Operation workflows** — step-by-step instructions for INGEST, QUERY, and LINT
- **Templates** — page templates for each page type (source, entity, concept, synthesis)
- **Log format** — the exact format for log entries, including the grep-parseable prefix
- **Session start protocol** — what to read and report at the start of every new session

The schema is co-owned by the human and the LLM. It evolves over time as the domain matures and workflows are refined. After each ingest or query session, if something didn't work as expected, the schema is the first thing to update.

## Relevance to This Wiki

This wiki's schema is `CLAUDE.md` at the root of `llm-wiki/`. It is the document currently governing all operations. It should be updated whenever a convention is changed, a new page type is added, or a workflow step is refined.

## Key Sources

[[LLM Wiki Pattern]] (source summary), [[Scaling Agentic Coding Across Your Organization]]

## Related Concepts

- [[LLM Wiki Pattern]] — the system the schema governs
- [[CLAUDE.md at Scale]] — the enterprise pattern for sharing CLAUDE.md across teams via git
- [[Knowledge Compounding]] — compounding only works consistently because the schema enforces it
- [[Agent Skills]] — shared CLAUDE.md functions like a team-level skill package
