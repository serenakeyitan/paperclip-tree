---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces comment-triggered agent wakes with debouncing, deduplication, and selective wake rules — a distinct architectural pattern not captured by any existing tree node.
---
# Comment Wake Protocol

How Paperclip wakes agents in response to comments on their assigned issues, and the efficiency optimizations that prevent redundant wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Architecture

When a comment is posted on an issue with an assigned agent, Paperclip can trigger an agent wake so the agent reads and responds. Comment wakes are event-driven and immediate, distinct from scheduled heartbeat wakes. They use the same adapter execution path but with comment-specific context delivery.

## Key Decisions

### Debouncing and Deduplication

Rapid successive comments are batched into a single wake. If the agent is already running (from a heartbeat or prior comment wake), duplicate wake triggers are suppressed or queued.

### Selective Wake Rules

Not every comment triggers a wake. System-generated comments, status change notifications, and the agent's own comments do NOT trigger wakes. Only substantive comments from other participants cause a wake. Comments on `in_review` issues can re-checkout the task so the agent addresses feedback without manual status changes.

### Server-Side Efficiency

All efficiency logic (debouncing, deduplication, selective filtering) lives server-side in backend services. Adapters receive a uniform wake signal regardless of trigger type — they are unaware of whether the wake originated from a heartbeat, comment, or dependency resolution.

## Boundaries

- The **heartbeat protocol** (scheduled wakes) is defined in `product/agent-model`. This node covers **comment-triggered wakes** specifically.
- The **task system** owns the comment model. This node covers the **wake mechanism** that reacts to comments.
- Execution workspace setup during a comment wake follows the worktree isolation pattern documented in `engineering/execution-workspaces`.
