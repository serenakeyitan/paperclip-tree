---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Worktree reseed (refresh existing worktree to new base in-place) is a new CLI operation with performance implications not captured in the tree.
---
# Worktree Reseed

A CLI operation that refreshes an existing git worktree to a new base branch in-place, rather than destroying and recreating it.

## Key Decisions

### In-Place Refresh

Reseed updates the worktree's base reference without destroying the worktree directory. This preserves local state (environment setup, cached builds, node_modules) that would be lost on recreation.

### Agent Wake Trigger

Reseed is triggered during agent wake when the worktree's current base is behind the target branch. This ensures agents always work against an up-to-date base without the latency cost of full worktree recreation.

### Performance Over Correctness Trade-off

Reseed is faster than recreation but may leave stale artifacts if the base change is dramatic (e.g., major refactors). For most incremental branch updates, this trade-off is worthwhile.
