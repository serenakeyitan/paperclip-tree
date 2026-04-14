---
title: "Comment Wake Protocol"
owners: [cryppadotta]
---

---
title: "Comment Wake Protocol"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/heartbeat-run-orchestration/NODE.md", "engineering/execution-workspaces/NODE.md", "product/task-system/NODE.md"]
---

# Comment Wake Protocol

How Paperclip wakes agents in response to comments on their assigned issues, and the efficiency optimizations that prevent redundant wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Key Decisions

### Comment Wakes Are a Separate Wake Trigger

Comment wakes coexist with scheduled heartbeats. They use the same adapter execution infrastructure but have independent triggering logic. Adapters receive a wake request and execute — they don't know whether it was triggered by a comment, heartbeat, or dependency resolution.

### Wake Efficiency Is Server-Side

The protocol includes efficiency measures to avoid redundant wakes:
- **Debouncing** — rapid successive comments are batched into a single wake.
- **Deduplication** — if the agent is already running, a new comment wake is suppressed or queued.
- **Selective wake** — system-generated comments, status change notifications, and comments from the agent itself do not trigger wakes.

All debouncing and deduplication logic lives in the backend services, not in adapters.

## Boundaries

- The **heartbeat protocol** (scheduled wakes) is defined in `product/agent-model`. This node covers **comment-triggered wakes** specifically.
- The **task system** owns the comment model. This node covers the **wake mechanism** that reacts to comments.
- **Execution workspace** setup during a comment wake follows the same worktree isolation pattern documented in the execution workspaces node.
