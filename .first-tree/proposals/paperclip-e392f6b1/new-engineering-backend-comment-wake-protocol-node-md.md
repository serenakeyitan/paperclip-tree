---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Comment-triggered agent wakes are a distinct architectural pattern with debouncing, deduplication, and selective wake rules — not captured anywhere in the tree.
---
# Comment Wake Protocol

Comment-triggered agent wakes are a distinct trigger type (event-driven, immediate) separate from heartbeat-based wakes. When a human or agent posts a comment on an issue, the assigned agent is woken to respond.

## Key Decisions

### Debouncing and Deduplication

Rapid successive comments are batched (debounced) to avoid redundant agent wakes. If the agent is already running, duplicate wake triggers are suppressed.

### Selective Wake Rules

Not all comments trigger wakes. System comments, status notifications, and the agent's own comments do NOT trigger wakes. Only substantive comments from other participants cause a wake.

### Server-Side Logic

All efficiency logic (debouncing, deduplication, selective filtering) lives server-side. Adapters are unaware of the wake trigger type — they receive a uniform wake signal regardless of whether it originated from a heartbeat or a comment.
