---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR enables agents to re-checkout in_review tasks when comment feedback arrives — a new wake-triggered checkout pattern not covered by the existing atomic checkout description in task-system/NODE.md.
---
# Wake Checkout and Worktrees

How agents reclaim tasks and resume work when new feedback arrives on issues they own.

## Key Decisions

### Comment-Triggered Re-Checkout

When a comment is posted on an issue in `in_review` status, the assigned agent can re-checkout the task to address feedback. This extends the existing atomic checkout model — the agent reclaims exclusive access without the task needing to be manually moved back to `todo`.

**Rationale:** Review cycles are the most common source of context loss. Allowing agents to wake on comment feedback keeps the feedback loop tight and avoids manual status juggling.

### Scoped Wake Protocol

The backend implements a comment wake protocol with debouncing and deduplication. Not every comment triggers a wake — only comments on issues where the assigned agent is idle and the issue is in a wake-eligible status (`in_review`). This prevents thrashing when multiple comments arrive in quick succession.

### Git Worktree Reuse

When an agent wakes to address feedback, it reuses the existing git worktree from its previous work on that issue. This preserves uncommitted changes and branch state, so the agent can pick up exactly where it left off rather than starting from a clean checkout.
