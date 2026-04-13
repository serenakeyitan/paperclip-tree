---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents atomic task checkout but not the scoped wake checkout flow or how git worktrees are linked to issues and reused across agent boot cycles — this PR improves both mechanisms.
---
# Wake Checkout & Linked Worktrees

How agents claim issues and manage git worktrees when they boot.

## Key Decisions

### Scoped Wake Checkout

When an agent wakes (via heartbeat or direct invocation), it calls the API to checkout the issue it has been assigned. This checkout is scoped — the agent claims exclusive access to the specific issue, preventing other agents from working on it concurrently. This builds on the atomic task checkout mechanism but operates at the agent boot boundary rather than at arbitrary task-claim time.

**Rationale:** Agents need to establish exclusive ownership of their work immediately upon waking, before they begin any code changes. Doing this at wake time rather than lazily prevents race conditions in multi-agent environments.

### Linked Worktree Reuse

Git worktrees are linked to issues. When an agent wakes to continue work on an issue it previously worked on, it reuses the existing worktree rather than creating a new one. This preserves uncommitted work, branch state, and local context from previous sessions.

**Rationale:** Creating fresh worktrees on every wake cycle wastes time and discards valuable context. Worktree reuse means agents can resume exactly where they left off, which is critical for multi-session tasks that span multiple heartbeat cycles.

## Boundaries

- Worktree creation and cleanup are adapter-side concerns (adapters manage the local filesystem).
- The server tracks the association between issues and worktree paths but does not manage the git operations directly.
