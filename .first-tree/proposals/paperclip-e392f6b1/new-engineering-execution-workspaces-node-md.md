---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The execution workspaces node covers worktree isolation and linked reuse but doesn't document runtime isolation hardening — environment propagation into worktrees, defensive setup/teardown on adapter crash, or orphaned worktree prevention introduced by this PR.
---
### Runtime Isolation Hardening

Worktree setup and teardown are defensive — validating that the target branch exists, the worktree path is writable, and cleanup runs even on adapter crash or timeout. This prevents orphaned worktrees from accumulating.

### Environment Propagation

Project-level environment variables and configuration (`.env`, project YAML) are explicitly propagated into the worktree context by the runtime rather than expecting adapters to discover env files themselves. This ensures agents have consistent configuration regardless of which adapter is in use.
