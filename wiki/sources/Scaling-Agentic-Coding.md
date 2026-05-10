---
type: source
tags: [ai-agents, coding, enterprise, rollout, claude-code, anthropic]
created: 2026-05-10
updated: 2026-05-10
sources: [scalingagents.pdf]
---

# Scaling Agentic Coding Across Your Organization

**File**: `raw/scalingagents.pdf`
**Type**: implementation guide
**Author(s)**: Anthropic
**Date**: August 2025
**Domain**: enterprise AI adoption, agentic coding, organizational change

## Summary

A 39-page Anthropic guide on scaling Claude Code from pilot group to full engineering organization. Covers rollout strategy, ROI measurement, adoption pitfalls, security, and innovative use cases. The central argument: successful agentic coding adoption is deliberate and phased, not a big-bang deployment. The organizations seeing the most success build internal expertise and infrastructure before scaling broadly. Includes a detailed appendix with CLAUDE.md templates, TDD examples, and prompt engineering guides.

## Key Claims

- [[Agentic Coding Rollout]] follows a deliberate expansion strategy — start with 20–50 power users who become internal champions, build expertise organically before scaling
- [[CLAUDE.md at Scale]] is the mechanism for aligning Claude Code across teams — checked into git, committed to main, included in onboarding, version-controlled like documentation; creates a "knowledge layer" for AI-powered development
- [[Test-Driven Agent Development]] solves the "do everything" trap — unbounded tasks produce poor results; writing tests first gives agents concrete guardrails and success criteria
- [[Agentic ROI Measurement]] should go beyond lines of code (a "notoriously spotty metric") — use sprint throughput, time-to-completion, migration acceleration, developer satisfaction, onboarding speed
- Security-by-design is foundational, not optional — encode security policies directly in CLAUDE.md as policy-as-code; use /security-review commands; sandbox MCP integrations before production ([[Dual-Use AI Risk]])
- Prompt engineering is a learnable skill — treat Claude like an engineer colleague; provide context-rich prompts with environment info, error details, reproduction steps, relevant code
- Innovative applications go beyond code generation: on-call automation, legacy migration specialists, non-technical contributor enablement (Brex, Rakuten examples)
- "Agentic coding is your team's cognitive multiplier" — human-guided, not human-replaced

## Key Entities

[[Anthropic]], [[Brex]], [[Rakuten]]

## Key Concepts

[[Agentic Coding Rollout]], [[CLAUDE.md at Scale]], [[Test-Driven Agent Development]], [[Agentic ROI Measurement]], [[CLAUDE.md Schema]], [[Dual-Use AI Risk]], [[Agentic SDLC]]

## Notable Quotes

> "The organizations seeing the most success aren't those who move fastest, but those who build the strongest foundations."

> "Agentic coding is your team's cognitive multiplier. The only question is: how quickly can you help them harness it?"

> "Security considerations aren't optional. They're foundational to successful agentic coding adoption."

> "Lines of code is a notoriously spotty metric."

> "Agentic coding represents a fundamental shift in how we build software, from human-written to human-guided development."

## Contradicts or Challenges

No direct contradictions. Complements and extends the other Anthropic sources:
- [[Building Effective AI Agents]] covers architecture patterns; this covers organizational rollout
- [[2026 Agentic Coding Trends Report]] covers trajectory; this covers implementation
- All three align on: start simple, human oversight is essential, security-first

## Questions Raised

- How do you manage CLAUDE.md drift when many teams contribute changes?
- What is the right cadence for MCP security audits in fast-moving orgs?
- How do you measure developer satisfaction in a way that captures genuine workflow improvement vs. novelty bias?
