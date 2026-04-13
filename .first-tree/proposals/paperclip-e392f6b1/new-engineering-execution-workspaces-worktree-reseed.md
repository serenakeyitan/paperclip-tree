---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3079 adds worktree reseed — re-initializing environment and project configuration into existing worktrees — which is not covered by the execution-workspaces node that only describes initial creation, reuse, and cleanup.
---
# Worktree Reseed

How existing git worktrees are re-seeded with fresh environment configuration and project state without being destroyed and recreated.

## Key Decisions

### Reseed Over Recreate

When a worktree's environment or project configuration becomes stale (e.g., after config changes, dependency updates, or credential rotation), the runtime re-seeds the existing worktree rather than creating a new one. This preserves in-progress work while ensuring the execution environment is current.

### Environment Propagation on Reseed

Project environment variables, credentials, and configuration files are re-propagated into the worktree during reseed. This ensures agents resuming work in a reused worktree operate with up-to-date context rather than stale configuration from the original creation.

### Defensive Reseed

Reseed validates that the target worktree is in a consistent state before applying updates. If the worktree has conflicting state, the reseed surfaces the conflict rather than silently overwriting.

## Boundaries

This is a sub-concern of [engineering/execution-workspaces](../NODE.md). Worktree creation, isolation, and lifecycle are documented in the parent node. CLI worktree commands are in [engineering/cli](../../cli/NODE.md).
