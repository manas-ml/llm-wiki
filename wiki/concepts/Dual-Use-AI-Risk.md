---
type: concept
tags: [security, ai-agents, risk, dual-use, safety]
created: 2026-05-10
updated: 2026-05-10
sources: [agentictrends.pdf]
---

# Dual-Use AI Risk

**Domain**: AI safety, cybersecurity, policy
**Also called**: dual-use risk, offensive/defensive AI, agentic security risk

## Definition

Dual-use AI risk is the security threat that emerges when the same AI capabilities that benefit defenders also benefit attackers. As agentic coding democratizes security expertise — enabling any engineer to perform security reviews, hardening, and monitoring — it simultaneously enables threat actors to scale offensive operations. The same tool, used in two directions.

## How It Works

### The symmetric threat

**Defensive benefits**
- Any engineer can perform in-depth security reviews previously requiring specialists
- Automated vulnerability detection and hardening at scale
- Security monitoring that would overwhelm human capacity
- Legacy system auditing and remediation

**Offensive risks**
- Threat actors use the same agents to scale attacks
- Automated vulnerability discovery and exploit generation
- Social engineering at scale
- Attack surface mapping at speed previously requiring large teams

### Why this is structurally different from prior security threats
Prior security tools required significant expertise to operate offensively. Agentic coding lowers that bar dramatically. A threat actor with access to capable coding agents can now leverage attack techniques that previously required specialized knowledge. The democratization cuts both ways — it doesn't just help defenders.

### Security-first architecture as prerequisite
The report frames security-first architecture not as a best practice but as a prerequisite for agentic systems. Key principles:
- Build security in from design, not retrofitted
- Assume agents will be used offensively; design defenses accordingly
- [[Human-in-the-Loop Oversight]] for security-sensitive agent actions
- Audit trails for all agent decisions (especially in long-running systems)

### Open question from source
The report states the threat exists and requires security-first architecture, but does not specify concrete architectural patterns for mitigating offensive use at scale. This is an active research and policy area.

## Key Sources

[[2026 Agentic Coding Trends Report]]

## Related Concepts

- [[Human-in-the-Loop Oversight]] — a key mitigation: human review of security-sensitive agent actions
- [[Agentic SDLC]] — the context in which dual-use risk manifests
- [[Long-Running Agent]] — autonomous agents increase the attack surface and audit complexity
- [[AI Agent]] — the capability that creates the dual-use dynamic
