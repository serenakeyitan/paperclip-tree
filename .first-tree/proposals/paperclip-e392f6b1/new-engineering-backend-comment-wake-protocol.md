---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents heartbeat-triggered wakes in product/agent-model but has no node for comment-triggered agent wakes — the debouncing, deduplication, and selective wake efficiency optimizations introduced by this PR.
---
# Comment Wake Protocol

How Paperclip wakes agents in response to comments on their assigned issues, and the efficiency optimizations that prevent redundant wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Architecture

### Comment-Triggered Wakes

When a comment is posted on an issue with an assigned agent, Paperclip triggers an agent wake so the agent reads and responds to the comment. This is distinct from scheduled heartbeat wakes — comment wakes are event-driven and immediate. The comment wake uses the same adapter execution path as heartbeat runs but with comment-specific context delivery (the new comment and its thread context).

### Wake Efficiency

Naive comment wakes are expensive: every comment would spawn an adapter execution. The comment wake protocol includes efficiency measures:
- **Debouncing** — rapid successive comments (e.g., during a human typing a multi-part message) are batched into a single wake.
- **Deduplication** — if the agent is already running (from a heartbeat or prior comment wake), a new comment wake is suppressed or queued.
- **Selective wake** — not every comment warrants a wake. System-generated comments, status change notifications, and comments from the agent itself do not trigger wakes.

## Key Decisions

- **Comment wakes are a separate wake trigger, not a heartbeat override.** They coexist with scheduled heartbeats, using the same execution infrastructure but independent triggering logic.
- **Efficiency is server-side.** Debouncing and deduplication logic lives in backend services, not in adapters. Adapters receive a wake request and execute without knowing the trigger type.

## Boundaries

- The **heartbeat protocol** (scheduled wakes, context delivery spectrum) is defined in `product/agent-model`. This node covers **comment-triggered wakes** specifically.
- The **task system** owns the comment model. This node covers the **wake mechanism** that reacts to comments.
- Execution workspace setup during a comment wake follows the same worktree isolation pattern documented in the worktree runtime node.
