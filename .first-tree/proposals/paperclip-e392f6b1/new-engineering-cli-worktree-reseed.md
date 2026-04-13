---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3079 adds worktree reseed — a mechanism to refresh an existing agent worktree to a new base without destroying and recreating it. No tree node or proposal mentions 'reseed'; existing worktree nodes cover only creation, reuse, and teardown.
---
# Worktree Reseed

Worktree reseed refreshes an existing agent worktree to a new base state without fully tearing down and recreating it. This preserves the worktree's identity and linkage to its issue while updating the underlying code to a newer branch point.

## Key Decisions

- **Reseed over recreate.** Rather than deleting an existing worktree and creating a new one when the base branch has advanced, reseed updates the worktree in place. This is faster and preserves local state (environment setup, cached builds) that would be lost on full recreation.
- **Triggered during agent wake.** Reseed is invoked as part of the agent wake cycle when the system detects the worktree's base is behind the target branch. This keeps agent workspaces current without manual intervention.
- **Complements linked worktree reuse.** Worktree reuse (see wake-checkout) preserves a worktree across sessions; reseed ensures the reused worktree stays current with upstream changes.

## Boundaries

The reseed operation is a CLI/adapter concern. The server tracks whether a reseed is needed based on branch state, but the actual git operations run locally in the adapter's environment.
