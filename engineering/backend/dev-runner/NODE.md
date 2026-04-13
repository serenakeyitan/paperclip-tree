---
title: "Dev Runner"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Dev Runner

The dev runner manages local execution workspaces for agents during development. It handles git worktree creation, environment isolation, and workspace lifecycle.

## Key Decisions

### Worktree Environment Isolation

Each execution workspace gets an isolated environment bootstrap — the dev runner does not leak environment variables or state from the parent process into worktree workspaces. This was tightened after discovering that shared env between the runner and worktrees caused subtle test and build failures.

### Linked Worktree Reuse

When an agent resumes work on an existing branch, the dev runner reuses the linked worktree rather than creating a new one. This avoids worktree proliferation and preserves uncommitted workspace state across agent wake cycles.

Source: `packages/server/` (execution workspace routes and dev runner modules).
