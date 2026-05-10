---
type: entity
tags: [framework, ai-agents, orchestration, python]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf]
---

# LangGraph

**Type**: product / software framework
**Also known as**: LangGraph (by LangChain)

## Overview

LangGraph is a Python framework for building stateful, multi-actor agent applications. It models agent workflows as graphs — nodes are agents or tools, edges define the flow of information and control. Mentioned in [[Building Effective AI Agents]] as a componentized agent framework offering natural integration points for new AI capabilities without system-wide refactoring.

## Key Facts

| Attribute | Value |
|-----------|-------|
| Type | Agent orchestration framework |
| Language | Python |
| Made by | LangChain |
| Paradigm | Graph-based stateful agent workflows |
| Use case | Building multi-agent systems with explicit control flow |

## Role in This Wiki

Cited as an example of a framework that implements componentized agent architecture — useful for comparing to lower-level Claude API approaches or the wiki's direct file-based agent pattern.

## Appears In

[[Building Effective AI Agents]]

## Related

- [[Multi-Agent Architecture]] — LangGraph is designed for multi-agent coordination
- [[Hierarchical Multi-Agent System]] — LangGraph supports supervisor patterns
- [[Sequential Workflow]], [[Parallel Workflow]] — expressible as graph structures in LangGraph
- [[Anthropic]] — Claude integrates with LangGraph
