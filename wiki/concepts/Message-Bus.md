---
type: concept
tags: [ai-agents, architecture, multi-agent, coordination, event-driven, pub-sub]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/multi-agent-coordination-patterns]
---

# Message Bus

**Domain**: AI agent architecture, multi-agent coordination
**Also called**: pub/sub agent coordination, event-driven agent pipeline, agent message queue

## Definition

The Message Bus is a multi-agent coordination pattern where agents publish and subscribe to topics through a central router. Agents are decoupled — publishers don't know which subscribers will act on their events, and subscribers don't know which publishers will produce them. New agents can be added to receive relevant work without rewiring existing connections, making the pattern well-suited to growing agent ecosystems with variable workflows.

## How It Works

```
Agent A → [publish: alert.high_severity] → Router → [subscribe: alert.high_severity] → Agent B
                                                   ↘ [subscribe: alert.*]             → Agent C
                                                   ↘ [subscribe: *.high_severity]     → Agent D
```

1. **Agents publish events** to named topics (e.g. `security.alert`, `code.review_needed`, `ticket.escalated`)
2. **Central router** receives events and routes them to all subscribed agents
3. **Subscriber agents** receive relevant events and take action, potentially publishing new events
4. **New agent types** can be added by subscribing to existing topics — no changes to existing agents required

### Strengths

- **Extensibility** — new capabilities added without modifying existing agents or rewiring connections
- **Decoupling** — agents develop independently; publishers and subscribers don't need to know about each other
- **Event-driven parallelism** — multiple agents can react to the same event simultaneously
- **Variable workflows** — the same event can trigger different agent chains depending on what's subscribed

### Weaknesses and failure modes

- **Silent failures** — router misclassification or dropped events fail without explicit error surfaces; a security alert that doesn't match any subscriber silently disappears
- **Cascade complexity** — one event can trigger chains of reactions across many agents; tracing a failure back to its root cause requires careful logging of the full event chain
- **Debugging difficulty** — non-deterministic routing (based on which agents are subscribed) makes reproduction of failures harder
- **Termination** — event-driven systems don't have a natural completion state; requires explicit termination conditions or time budgets

### When to use vs. alternatives

| Choose Message Bus when... | Choose instead when... |
|---------------------------|----------------------|
| Workflows emerge from events and vary based on discoveries | Workflow steps are predetermined → [[Hierarchical Multi-Agent System]] |
| Agent ecosystem is growing and new types will be added | Agent roster is fixed → [[Parallel Workflow]] or [[Agent Teams]] |
| Multiple agents need to react to the same event | One agent handles each event type → [[Sequential Workflow]] |
| Discrete event reactions rather than accumulated state | Agents accumulate findings over time → [[Shared State]] |

### Example use case

Security operations automation: a threat detection agent publishes `alert.potential_intrusion`. The router delivers this to a log analysis agent (subscribed to `alert.*`), an IP reputation agent (subscribed to `alert.potential_*`), and a ticketing agent (subscribed to `*.potential_intrusion`). Each responds independently. If the log analysis agent finds confirmed suspicious activity, it publishes `alert.confirmed_intrusion`, triggering a different downstream chain. New agent types (e.g. a compliance notification agent) can be added by subscribing to relevant topics without touching any existing agents.

## Combining with Other Patterns

Message Bus pairs naturally with [[Agent Teams]]: the bus routes incoming work to a team of persistent workers, each of which handles events over multiple steps. The bus handles routing and extensibility; the team handles sustained execution.

## Relevance to This Wiki

The wiki's operation system (INGEST, QUERY, LINT) could be reimplemented as a message bus: a human publishes `wiki.ingest_requested` with a source URL; a reader agent fetches content and publishes `wiki.content_ready`; a writer agent picks up content-ready events and creates pages; an index agent picks up page-created events and updates index.md. This would decouple the operations but significantly complicate debugging compared to the current linear workflow.

## Key Sources

[[Multi-Agent Coordination Patterns]]

## Related Concepts

- [[Hierarchical Multi-Agent System]] — alternative for predetermined, orchestrated workflows
- [[Agent Teams]] — complementary pattern; bus routes work to persistent workers
- [[Shared State]] — alternative when agents need accumulated findings rather than discrete events
- [[Multi-Agent Architecture]] — the broader category; Message Bus is one of five named coordination patterns
- [[Dual-Use AI Risk]] — event-driven silent failures are an especially hard-to-audit attack surface
- [[Durable Agent Execution]] — event tracing and observability are essential for message bus systems
