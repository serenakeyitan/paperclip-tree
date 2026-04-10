---
title: "Agent Model"
owners: [cryppadotta, serenakeyitan]
soft_links: ["product/governance/NODE.md", "product/task-system/NODE.md", "adapters/NODE.md", "engineering/NODE.md"]
---

# Agent Model

How agents (employees) are defined, organized, and coordinated within a Paperclip company.

## Key Decisions

### Every Employee is an Agent

There is no distinction between "AI agent" and "employee" -- the terms are synonymous. When you create a company, you define the CEO, then build out from there. This is the core metaphor that makes Paperclip feel like running a company rather than managing a fleet of bots.

### Adapter-Defined Identity

Agent identity and behavior (e.g., SOUL.md, HEARTBEAT.md, CLAUDE.md) are **not part of the Paperclip protocol**. They are adapter-specific configurations. Paperclip tracks the control plane fields (id, name, role, title, status, org position, adapter type + config blob). The adapter defines what config fields exist and how the agent's inner workings operate.

**Rationale:** This separation keeps Paperclip runtime-agnostic. An OpenClaw agent, a Claude Code agent, and a bare Python script all fit the same protocol. Paperclip doesn't need to understand agent internals to orchestrate them.

### Minimum Contract: Be Callable

The only requirement to be a Paperclip agent is that Paperclip can invoke it (via command or webhook). No requirement to report back. Beyond that, three progressive integration levels:

1. **Callable** -- Paperclip can start you (minimum)
2. **Status reporting** -- Agent reports success/failure/in-progress
3. **Fully instrumented** -- Agent reports status, cost/token usage, task updates, and logs

**Rationale:** Low barrier to entry means any existing tool can become a Paperclip agent. Progressive integration rewards deeper adoption without punishing simple use cases.

### Strict Tree Org Structure (V1)

`reports_to` forms a strict tree with a nullable root (CEO). No multi-manager reporting in V1. The org structure defines **reporting and delegation lines**, not access control -- every agent has full visibility across the entire org.

**Rationale:** Trees are simple to reason about for both humans and agents. Multi-manager support adds complexity without clear V1 value. Full visibility enables agents to discover collaborators by reading capabilities descriptions.

### Cross-Team Coordination via Tasks

Agents can create tasks and assign them to agents outside their reporting line. Task acceptance rules are primarily encoded in the Paperclip SKILL.md (agent behavior) rather than enforced by the platform:

1. **Agrees + can do it** -- complete directly
2. **Agrees + can't do it** -- mark blocked
3. **Questions whether it's worth doing** -- cannot cancel; must reassign to own manager for decision

Managers are responsible for understanding why subordinates are blocked and resolving it (delegate down, escalate up).

**Rationale:** Hard enforcement of cross-team rules would make the system too rigid. Encoding them as agent norms (via SKILL.md) keeps the system flexible while establishing clear expectations.

### Request Depth Tracking

Cross-team tasks track an integer **depth** counting delegation hops from the original requester. This provides visibility into how far work cascades through the org.

### Billing Codes for Cost Attribution

Tasks carry a **billing code** so token spend is attributed upstream to the requesting task/agent. When Agent A asks Agent B to do work, B's costs roll up to A's request.

### Heartbeat Protocol

The heartbeat is a protocol, not a runtime. Paperclip controls **when** to fire (schedule/frequency), **how** to fire (adapter selection), and **what context** to include (thin ping vs. fat payload). Paperclip does **not** control how long the agent runs, what it does during its cycle, or whether it's task-scoped/time-windowed/continuous.

Pause behavior: graceful signal to current execution, grace period, force-kill after timeout, stop future heartbeats.

### Context Delivery Spectrum

Configurable per agent between two poles:
- **Fat payload** -- Paperclip bundles relevant context into heartbeat invocation (for simple/stateless agents)
- **Thin ping** -- wake-up signal only; agent fetches what it needs via API (for sophisticated agents)

### Default Agents

Paperclip ships default agent templates:
- **Default Agent** -- Claude Code/Codex loop with Paperclip Skill for task system interaction
- **Default CEO** -- extends default with strategic planning, delegation, board communication

These are starting points and reference implementations, not mandated patterns.

## Decision Records

- [adapter-defined-agent-internals.md](adapter-defined-agent-internals.md) — Why Paperclip owns control-plane agent fields while adapters own runtime-specific behavior.

## Agent Statuses

`active`, `paused`, `idle`, `running`, `error`, `terminated`. Terminated is irreversible (board only).

## Open Questions

- Can org structure change at runtime (agents reassigned, teams restructured)?
- Do agents inherit any configuration from their manager?
- Health monitoring -- distinguishing "stuck" from "working on a long task"
- Can agents self-trigger their next heartbeat?
