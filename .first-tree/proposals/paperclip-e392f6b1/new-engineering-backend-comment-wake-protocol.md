---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces comment-triggered agent wakes with debouncing, deduplication, and selective wake filtering — a distinct wake mechanism from heartbeat runs that no existing node covers.
---
# Comment Wake Protocol

Event-driven agent wakes triggered by comments on issues, distinct from scheduled heartbeat wakes.

**Source:** `server/src/services/` (comment-related wakeup logic), `server/src/routes/issues/` (comment endpoints).

## Key Decisions

### Event-Driven, Not Polled

Comment-triggered agent wakes are immediate and event-driven, not batched into heartbeat cycles. When a comment is posted on an issue assigned to an agent, the agent is woken immediately rather than waiting for the next scheduled heartbeat.

**Rationale:** Comments often represent urgent context changes — a human asking a question, a blocker being discussed, new requirements. Waiting for the next heartbeat cycle introduces unacceptable latency in interactive workflows.

### Debouncing and Deduplication

Rapid sequential comments are debounced into a single wake. If an agent is already running, duplicate wake requests are suppressed. System-generated comments (status changes, automated notifications) and comments from the agent itself do not trigger wakes.

**Rationale:** Without these guards, a burst of comments would spawn redundant agent runs, wasting compute and potentially causing conflicts.

### Shared Execution Infrastructure

Comment wakes use the same execution infrastructure as heartbeat runs but inject comment-specific context into the agent's wake payload. This avoids duplicating the run lifecycle machinery. The efficiency logic (debouncing, deduplication) lives server-side — adapters receive a wake request and execute without knowing the trigger type.

## Boundaries

- The **heartbeat protocol** (scheduled wakes, context delivery) is in `engineering/backend/heartbeat-run-orchestration`. This node covers **comment-triggered wakes** specifically.
- The **task system** owns the comment data model. This node covers the **wake mechanism** that reacts to comments.
- Execution workspace setup during a comment wake follows the same worktree isolation pattern in `engineering/execution-workspaces`.
