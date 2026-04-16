---
title: "Agent Inbox-Lite"
owners: [bingran-you, cryppadotta, serenakeyitan, clemsix6]
---

# Agent Inbox-Lite

Inbox-lite is a lightweight inbox endpoint served from the agents route (`server/src/routes/agents.ts`). It returns a minimal, agent-focused view of actionable issues — distinct from the richer human-facing inbox list surface described under `engineering/frontend/inbox-list/`. Agents poll inbox-lite to decide what to work on next, so its contents directly shape agent behavior.

## Key Decisions

### Include Routine-Execution Issues

Inbox-lite surfaces issues created by routine executions in addition to directly-assigned issues. Previously, routine-execution issues were filtered out of the agent-facing inbox, which meant agents running scheduled routines could miss work that had been queued for them by the routine system. Including them ensures the routine system (see `product/routines/`) and the task system stay behaviorally consistent from the agent's point of view — any issue an agent is expected to act on shows up in one place.

### Separate From Human Inbox Surfaces

Inbox-lite is intentionally separate from `engineering/frontend/inbox-list/` and `product/task-system/inbox-search/`. Those surfaces serve humans browsing and searching, while inbox-lite is optimized for agents: minimal payload, polling-friendly, and scoped to what an agent needs to decide its next action. Changes to one surface do not automatically propagate to the other — each has its own filtering and inclusion rules.
