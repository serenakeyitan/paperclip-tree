---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits isolate and harden the dev runner's worktree environment bootstrap and linked worktree reuse, but no tree node documents the dev runner subsystem at all.
---
# Dev Runner

The dev runner manages local execution workspaces for agents during development. It handles git worktree creation, environment isolation, and workspace lifecycle.

## Key Decisions

### Worktree Environment Isolation

Each execution workspace gets an isolated environment bootstrap — the dev runner does not leak environment variables or state from the parent process into worktree workspaces. This was tightened after discovering that shared env between the runner and worktrees caused subtle test and build failures.

### Linked Worktree Reuse

When an agent resumes work on an existing branch, the dev runner reuses the linked worktree rather than creating a new one. This avoids worktree proliferation and preserves uncommitted workspace state across agent wake cycles.

Source: `packages/server/` (execution workspace routes and dev runner modules).
