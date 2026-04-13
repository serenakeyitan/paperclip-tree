---
title: "Worktree Runtime"
owners: []
---

## Worktree Runtime

Git worktrees are used as isolated execution workspaces for agent issue work. The runtime manages worktree lifecycle, environment setup, and tooling.

### Key Concepts

- **Worktree-per-issue** — Each issue checkout creates a git worktree, providing filesystem isolation between concurrent agent tasks.
- **Worktree reseed** — A CLI command (`worktree reseed`) resets worktree state for reuse without full recreation.
- **Environment isolation** — Dev runner worktree environments are isolated to prevent cross-contamination between agent runs. Environment bootstrap is tightened to scope variables correctly.
- **Node modules provisioning** — Local `node_modules` are provisioned in issue worktrees to ensure dependencies are available without polluting the main workspace.
- **Workspace link preflight** — Preflight checks validate workspace links in worktrees. Scoped to linked worktrees only (not all workspaces) and runs through the server toolchain.

### Decisions

- Worktrees over branches or containers for isolation — lighter weight than containers, better isolation than branch switching.
- Environment isolation is enforced at bootstrap time, not just at runtime, to catch misconfiguration early.
- Workspace link preflight hooks were removed in favor of scoped preflight through the server toolchain.
