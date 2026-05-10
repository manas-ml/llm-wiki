---
type: concept
tags: [enterprise, rollout, adoption, change-management, ai-agents]
created: 2026-05-10
updated: 2026-05-10
sources: [scalingagents.pdf]
---

# Agentic Coding Rollout

**Domain**: enterprise AI adoption, organizational change management
**Also called**: agentic coding adoption, Claude Code rollout, phased AI deployment

## Definition

Agentic Coding Rollout is the deliberate, phased process of expanding agentic coding tools from a small pilot group to an entire engineering organization. The key insight: successful rollouts build expertise and enthusiasm organically through internal champions, not through top-down mandates or big-bang deployments.

## How It Works

### Phase 1: Power user pilot (20–50 developers)
Choose developers already comfortable with AI-assisted tools — not just early adopters, but future champions. Task them with:
- Using the tool for common use cases to discover useful customizations
- Creating **custom slash commands** tailored to your codebase (e.g. `/migrate-db`, `/add-feature`, `/fix-security`)
- Writing [[CLAUDE.md at Scale|CLAUDE.md files]] with coding standards, gotchas, and best practices
- Documenting what works and what doesn't

### Phase 2: Build infrastructure for scale
Before expanding, ensure:
- CLAUDE.md files are checked into git and committed to main
- Custom slash commands are documented and shareable
- ROI measurement baselines are established ([[Agentic ROI Measurement]])
- Security policies are encoded ([[Dual-Use AI Risk]])
- Onboarding materials include CLAUDE.md review

### Phase 3: Expand via champions
Use pilot group members as champions who train and support wider teams. Run hackathons to generate enthusiasm and surface innovative use cases. Expand in waves rather than all at once.

### Phase 4: Scale deliberately
Correlate adoption metrics with business outcomes. Add complexity (multi-agent workflows, custom SDK agents) only when foundations are solid.

### Common pitfalls to avoid
| Pitfall | Symptom | Fix |
|---------|---------|-----|
| "Do everything" trap | Unbounded tasks, poor results | [[Test-Driven Agent Development]] |
| Poor prompt quality | Inconsistent output | Prompt engineering training |
| Security afterthought | Vulnerabilities in generated code | Policy-as-code in CLAUDE.md |
| No measurement baseline | Can't prove ROI | Define [[Agentic ROI Measurement]] metrics before pilot |
| Big-bang deployment | Adoption resistance, inconsistent results | Phased rollout with champions |

## Relevance to This Wiki

The [[LLM Wiki Pattern]] follows the same rollout logic: start with a minimal viable schema (CLAUDE.md), test it with a few ingests, refine based on what doesn't work, then scale to more sources and more complex queries. The phased approach prevents over-engineering before you know what works.

## Key Sources

[[Scaling Agentic Coding Across Your Organization]]

## Related Concepts

- [[CLAUDE.md at Scale]] — the technical mechanism that makes rollout stick across teams
- [[Test-Driven Agent Development]] — solves the most common adoption pitfall
- [[Agentic ROI Measurement]] — how to demonstrate value during and after rollout
- [[Agentic SDLC]] — what the rollout is building toward
- [[Engineer as Orchestrator]] — the role transformation the rollout enables
- [[Dual-Use AI Risk]] — security considerations that must be addressed during rollout
