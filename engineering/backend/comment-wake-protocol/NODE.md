---
title: "Comment Wake Protocol"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Comment Wake Protocol

How Paperclip wakes agents in response to comments on their assigned issues, and the efficiency optimizations that prevent redundant wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Key Decisions

### Comment-Triggered Wakes Are Event-Driven

When a comment is posted on an issue with an assigned agent, Paperclip triggers an agent wake so the agent reads and responds. This is distinct from scheduled heartbeat wakes — comment wakes are immediate and event-driven. The comment wake uses the same adapter execution path as heartbeat runs but with comment-specific context delivery.

### Wake Efficiency Optimizations

Naive comment wakes are expensive. The protocol includes:
- **Debouncing** — rapid successive comments are batched into a single wake.
- **Deduplication** — if the agent is already running, a new comment wake is suppressed or queued.
- **Selective wake** — system-generated comments, status change notifications, and comments from the agent itself do not trigger wakes.

### Separate Trigger, Shared Infrastructure

Comment wakes coexist with scheduled heartbeats. They use the same execution infrastructure but have independent triggering logic. Efficiency logic lives server-side — adapters don't know what triggered their wake.

## Boundaries

- The heartbeat protocol (scheduled wakes) is defined in `engineering/backend/heartbeat-run-orchestration/`.
- The task system owns the comment model. This node covers the wake mechanism that reacts to comments.
- Execution workspace setup during a comment wake follows the worktree isolation pattern in `engineering/execution-workspaces/`.
