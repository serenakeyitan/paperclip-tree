---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing execution-workspaces node covers worktree isolation and reuse but not the runtime hardening introduced by this PR: defensive setup/teardown, environment propagation into worktrees, and re-propagation on reuse.
---
# Worktree Runtime Hardening

How Paperclip hardens the setup and teardown of git worktree execution environments, and how project-level environment configuration propagates into agent workspaces.

**Source:** `server/src/services/execution-workspace*`, CLI `worktree` command, related runtime setup code.

## Key Decisions

### Defensive Setup and Teardown

Worktree creation and cleanup are hardened against failure modes: the runtime validates that the target branch exists, the worktree path is writable, and required configuration is present before handing the workspace to an adapter. Cleanup runs unconditionally — even on adapter crash, timeout, or unexpected termination — to prevent orphaned worktrees from accumulating on disk.

### Project Environment Propagation

Environment variables, credentials, and project configuration (`.env` files, YAML config) are explicitly propagated into the worktree context during setup rather than relying on adapters to discover or inherit them from the parent checkout. This ensures agents in worktrees operate with the correct, complete environment regardless of how the worktree was created or reused.

### Re-propagation on Reuse

When a worktree is reused across agent wake cycles, the runtime re-propagates environment configuration to ensure the agent operates with current values. This handles credential rotation, config changes, and dependency updates without requiring worktree recreation.

## Boundaries

- This is a sub-concern of [engineering/execution-workspaces](../NODE.md). Worktree creation, isolation model, and lifecycle are in the parent node.
- CLI worktree commands are in [engineering/cli](../../cli/NODE.md).
- Container-level isolation and network sandboxing belong to infrastructure/deployment.
