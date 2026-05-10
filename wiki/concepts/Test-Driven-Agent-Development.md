---
type: concept
tags: [tdd, ai-agents, coding, testing, guardrails, prompt-engineering]
created: 2026-05-10
updated: 2026-05-10
sources: [scalingagents.pdf]
---

# Test-Driven Agent Development

**Domain**: software engineering, AI agent usage
**Also called**: TDD with agents, agent-guided TDD, test-first agentic coding

## Definition

Test-Driven Agent Development (TDAD) is the practice of using test-driven development as guardrails for agentic coding tools. Instead of giving agents large, unbounded tasks — which produces poor results — developers write tests first to define concrete success criteria, then have the agent implement just enough code to pass one test at a time. Tests provide the structure that prevents agents from going off-track.

## How It Works

### The problem: the "do everything" trap
New users give agents massive, vague tasks: "Build a user authentication system." Agents are enthusiastic but without context or constraints they produce inconsistent, hard-to-validate output. This is the most common adoption failure mode.

### The solution: tests as specifications

**Step 1: Define requirements via tests**
```bash
claude "Create comprehensive tests for a user authentication system 
with registration, login, password reset, and JWT token management"
```
The agent writes test cases defining exactly what success looks like — authentication flows, edge cases, error handling, security requirements.

**Step 2: Run tests (they should fail)**
```bash
pytest tests/test_auth_system.py -v
# Expected: All tests fail — implementation doesn't exist yet
```
Failing tests confirm the spec is correct and implementation is genuinely absent.

**Step 3: Implement incrementally**
```bash
claude "Implement the AuthSystem class to pass all existing tests. 
Focus on security best practices for password hashing and JWT token management."
```
Agent implements just enough to pass the tests. No scope creep — the tests define the boundary.

**Step 4: Iterate**
Run tests, fix failures, add new test cases for edge cases discovered during implementation.

### Why this works
- Tests provide **concrete success criteria** — the agent has a clear target
- Incremental implementation **bounds scope** — one test at a time prevents sprawl
- Failing tests **validate the spec** before any implementation occurs
- Tests become **living documentation** of what the system should do

### Prompt engineering principles (from guide)
Beyond TDD, context-rich prompts produce dramatically better results:
- **Treat Claude like an engineer** — would a teammate understand exactly what you're asking?
- **Include environment info**: OS, runtime versions, framework, dependencies
- **Provide full error context**: stack traces, browser console errors, reproduction steps
- **Share relevant code files**: Claude can't debug code it can't see
- **Specify expected vs. actual behavior**: "Expected: 200 with user data. Actual: 401 with 'invalid token'"

## Relevance to This Wiki

The wiki's INGEST workflow applies the same principle: the [[CLAUDE.md Schema]] specifies exactly what "success" looks like for each operation (6 steps for ingest, 5 for query, 6 for lint), giving Claude concrete criteria rather than a vague task. The schema is the "test suite" for wiki operations.

## Key Sources

[[Scaling Agentic Coding Across Your Organization]]

## Related Concepts

- [[Agentic Coding Rollout]] — TDAD solves the most common adoption pitfall
- [[Human-in-the-Loop Oversight]] — tests are a form of automated oversight, reducing the burden on human reviewers
- [[Evaluator-Optimizer Workflow]] — tests serve as the evaluator in an automated generate-evaluate-refine loop
- [[CLAUDE.md Schema]] — the wiki's equivalent: concrete operational specs rather than vague instructions
