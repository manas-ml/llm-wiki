---
type: concept
tags: [ai-agents, skills, architecture, modularity, domain-expertise, context-engineering]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf, https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work]
---

# Agent Skills

**Domain**: AI agent architecture, domain expertise packaging
**Also called**: skill packages, capability modules, agent expertise packages

## Definition

Agent Skills are organized file collections that package domain expertise, workflows, and best practices in a format agents can access and apply. They bridge the gap between an agent's general reasoning capability and the accumulated professional knowledge required for specialized work. The core insight: **general capability ≠ domain expertise**. An agent with brilliant reasoning can solve any problem from first principles, but an experienced professional who has done it thousands of times is faster, more reliable, and catches edge cases the reasoner misses.

The tax professional analogy: a math genius might solve your tax problems from first principles, but most people prefer an experienced tax professional who has filed thousands of returns.

## Structure of a Skill

A skill is a directory containing:

```
skill_name/
├── SKILL.md          # Primary skill file: instructions, usage guide, overview
├── docs.md           # Supporting reference documentation
├── reference.md      # Deep-reference material loaded on demand
└── apply_template.py # Executable scripts agents can invoke or modify
```

**Skills are plain files** — version-controlled via Git, storable in cloud services, shareable across teams. Non-engineers (product managers, analysts, domain experts) can create and iterate on skills in ~30 minutes using interactive skill-creator tools.

## Three-Tier Progressive Disclosure

Skills use progressive disclosure to protect context windows while enabling large skill libraries:

| Tier | Size | When loaded | Contents |
|------|------|-------------|----------|
| **Metadata** | ~50 tokens | Always at runtime | YAML frontmatter: name + description only |
| **Full skill** | ~500 tokens | When Claude determines the skill is needed | Complete SKILL.md with instructions and usage |
| **Reference** | 2,000+ tokens | On demand during task execution | Supporting docs, detailed reference material |

**Example YAML frontmatter** (metadata tier):
```yaml
---
name: Anthropic Brand Style Guidelines
description: Anthropic's official brand colors, typography, and slide templates for presentations
---
```

This three-tier approach enables equipping agents with hundreds of skills — the agent sees all skill descriptions in ~50 tokens each, loads only what's relevant, and pulls deep reference only when needed. This is [[Context Engineering]] applied to expertise delivery: the smallest possible token set at each stage.

## Skills Include Executable Scripts

Skills can package executable scripts alongside documentation. This is a key advantage over traditional tool definitions:

| Dimension | Traditional tool | Skill-packaged script |
|-----------|-----------------|----------------------|
| Documentation | Written separately, can drift | Co-located with code — always in sync |
| Modifiability | Agent cannot change it | Agent can read, adapt, and extend it |
| Context load | Always present in context | Loaded on demand |
| Maintenance | Requires engineer to update | Can be updated by domain expert or agent |

**Example** (from the article): Claude repeatedly wrote identical scripts applying Anthropic brand styling to presentations. The solution was saving this as a reusable skill component:

```python
# anthropic/brand_styling/apply_template.py
import sys
from pptx import Presentation

if len(sys.argv) != 2:
    print("USAGE: apply_template.py <pptx>")
    sys.exit(1)

prs = Presentation(sys.argv[1])
for slide in prs.slides:
    ...
```

The SKILL.md documents that the script exists and when to use it; the script itself is self-documenting code. The agent doesn't have to reinvent this logic each time.

## The Skills Ecosystem

Three types of skills have emerged:

| Type | Who builds it | What it packages | Examples |
|------|--------------|-----------------|---------|
| **Foundational** | Community/Anthropic | Core capabilities everyone needs | Document generation, spreadsheet manipulation, presentation creation |
| **Partner** | Third-party companies | Their services made agent-accessible | K-Dense, Browserbase, Notion integrations |
| **Enterprise** | Internal teams | Proprietary processes, compliance, institutional knowledge | Internal compliance workflows, company-specific reporting formats |

### Complexity spectrum

Skills range from simple to highly complex:

| Complexity | Size | Example |
|------------|------|---------|
| Simple | ~100 lines | Status report writer — templates and formatting |
| Intermediate | ~800 lines | Financial model builder — data retrieval + Excel modeling with Python |
| Complex | 2,500+ lines | RNA sequencing pipeline — coordinates HISAT2, StringTie, DESeq2 |

## Skills and MCP are Complementary

Skills and MCP servers work together naturally — a skill can coordinate multiple MCP connections alongside its own documentation and scripts:

```
competitive_analysis/
├── SKILL.md          # How to conduct competitive analysis
├── methodology.md    # Framework and evaluation criteria
└── (uses MCP: web_search, internal_db, slack, notion)
```

MCP handles connectivity to external services; Skills package the expertise for how to use those connections effectively.

## The Four-Layer Agent Architecture

Skills complete a four-layer architecture for production agents:

```
┌─────────────────────────────────┐
│  Agent Loop (reasoning)         │ — Decides what to do next
├─────────────────────────────────┤
│  Agent Runtime (execution)      │ — Code, bash, filesystem
├─────────────────────────────────┤
│  MCP Servers (connectivity)     │ — External tools and data
├─────────────────────────────────┤
│  Skills Library (expertise)     │ — Domain knowledge + workflows
└─────────────────────────────────┘
```

Each layer has a distinct purpose and evolves independently:
- **Loop**: the Claude model — reasons about next actions
- **Runtime**: [[Claude Agent SDK]] — executes code and filesystem operations
- **MCP**: connections to Slack, GitHub, Asana, databases
- **Skills**: what makes the agent genuinely expert rather than merely capable

## Skills as an Open Standard

Agent Skills are announced as an open standard — similar to how MCP standardized agent connectivity. The same skill should function whether using Claude or other AI platforms. This enables:
- Skills built by domain experts to be shared across organizations
- Partner skills to work across platforms without reimplementation
- A collective knowledge base that benefits the whole ecosystem

## Relevance to This Wiki

The [[CLAUDE.md Schema]] is this wiki's instance of the Skills pattern: it packages the ingest/query/lint workflows, templates, and conventions as a structured file the agent loads each session. The Session Start Protocol (load CLAUDE.md + index.md + last log entries) follows the same progressive disclosure logic — stable expert context loads upfront; dynamic content (specific wiki pages) loads on demand during query.

## Key Sources

[[Building Effective AI Agents]], [[Building Agents with Skills: Equipping Agents for Specialized Work]]

## Related Concepts

- [[Single-Agent Architecture]] — Skills are what make a single agent powerful enough for complex tasks before reaching for multi-agent
- [[Context Engineering]] — three-tier progressive disclosure is context engineering applied to expertise delivery
- [[Just-in-Time Context Retrieval]] — reference tier loading is JIT applied to skill documentation
- [[CLAUDE.md Schema]] — the wiki's equivalent of a skill package
- [[CLAUDE.md at Scale]] — shared CLAUDE.md functions like a team-level skill; skills formalize this
- [[Multi-Agent Architecture]] — skills can be distributed across specialist agents
- [[Claude Agent SDK]] — the runtime layer that sits below the skills layer in the four-layer architecture
