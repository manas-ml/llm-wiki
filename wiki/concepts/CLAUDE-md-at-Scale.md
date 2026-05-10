---
type: concept
tags: [claude-code, claude-md, enterprise, configuration, knowledge-management]
created: 2026-05-10
updated: 2026-05-10
sources: [scalingagents.pdf]
---

# CLAUDE.md at Scale

**Domain**: enterprise AI tooling, configuration management
**Also called**: shared CLAUDE.md, team CLAUDE.md, organizational CLAUDE.md

## Definition

CLAUDE.md at Scale describes the practice of treating CLAUDE.md configuration files as shared, version-controlled team infrastructure rather than personal configuration. When checked into git and distributed across teams, CLAUDE.md becomes a "knowledge layer" that aligns every Claude Code session to team standards, coding conventions, and organizational context — compounding in value as teams contribute to it.

## How It Works

### From personal to shared
A personal CLAUDE.md helps one developer. A shared CLAUDE.md helps every developer who touches that codebase:

| Practice | Personal CLAUDE.md | Shared CLAUDE.md |
|----------|-------------------|-----------------|
| Location | Local only | Repository root, committed to main |
| Scope | One developer's sessions | Every developer who clones the repo |
| Updates | Ad hoc | Versioned like documentation |
| Onboarding | Not part of process | Reviewed as part of developer onboarding |

### Best practices for team CLAUDE.md

**Check into git and commit to main**: Every developer who clones the repository automatically inherits the project's Claude Code configuration. No manual setup.

**Include in onboarding checklists**: New team members should understand the CLAUDE.md file — not just the codebase, but how Claude Code should be used within the project context.

**Version control like documentation**: CLAUDE.md changes should go through the same review process as documentation changes. PRs, reviews, changelog entries for significant changes.

**Policy-as-code**: Encode security and quality policies directly in CLAUDE.md so Claude enforces them automatically:
```
- All database queries must use parameterized statements
- API endpoints require authentication middleware
- No hardcoded secrets in configuration files
- All new modules use TypeScript
- Run tests before committing: npm test
```

**Architecture notes**: Include the decisions that aren't obvious from the code — why Redis is used for queuing, why connection pooling is required, what the current sprint focus is.

### Example structure (from the guide)
```markdown
# Project Feline - Cloud Task Scheduler

## Development Environment
- Use Node.js 18+ with nvm
- Run `npm run setup` for initial environment configuration
- All database changes require migration files in /migrations

## Claude Code Guidelines
- Always run tests before committing: `npm test`
- Use TypeScript for all new modules
- Security: Never commit API keys or database credentials

## Architecture Notes
- Task scheduling uses Redis queues
- All external API calls must include retry logic and timeouts
- Database queries should use connection pooling

## Current Sprint Focus
- Implementing distributed task execution
- Focus on fault-tolerance and recovery patterns
```

## Connection to [[CLAUDE.md Schema]]

The [[CLAUDE.md Schema]] in this wiki is an instance of the same pattern: a shared configuration file that governs all LLM sessions working on the wiki. The difference is scope — the wiki's CLAUDE.md governs knowledge management operations (ingest, query, lint); an engineering team's CLAUDE.md governs coding operations (style, security, architecture).

Both follow the same principle: encode what the LLM needs to know once, and it applies consistently across every session.

## Key Sources

[[Scaling Agentic Coding Across Your Organization]]

## Related Concepts

- [[CLAUDE.md Schema]] — the wiki's instance of this pattern
- [[Agentic Coding Rollout]] — CLAUDE.md is the infrastructure that makes rollout scale
- [[Dual-Use AI Risk]] — policy-as-code in CLAUDE.md is a key security mechanism
- [[Agent Skills]] — shared CLAUDE.md functions like a team-level skill package
