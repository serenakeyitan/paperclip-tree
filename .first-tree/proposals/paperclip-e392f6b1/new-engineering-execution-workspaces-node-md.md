---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The execution-workspaces node covers worktree isolation between agents but does not address runtime isolation improvements within a single agent's worktree execution environment.
---
### Worktree Runtime Isolation

Beyond file-level isolation via separate git worktrees, agent execution environments enforce runtime isolation — ensuring that processes, environment variables, and temporary state from one execution do not leak into subsequent executions in the same worktree. This is critical for worktree reuse (where the same worktree is reused across wake cycles) to prevent state pollution between runs.

**Rationale:** Worktree reuse preserves valuable context (uncommitted changes, build artifacts) but risks leaking runtime state (orphan processes, stale environment). Runtime isolation draws the line: filesystem state is preserved, runtime state is reset.
