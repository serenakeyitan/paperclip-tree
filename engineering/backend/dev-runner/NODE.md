---
title: "Dev Runner & Execution Workspaces"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Dev Runner & Execution Workspaces

The dev runner manages git worktree-based execution workspaces where agents perform their work. Each agent run can operate in an isolated worktree rather than the main checkout, preventing conflicts between concurrent agents.

## Key Decisions

### Worktree Environment Isolation

Each worktree-based execution workspace bootstraps its own isolated environment. Environment variables, paths, and workspace state are scoped to the worktree, not inherited from the parent process. This was hardened in PR #3386 to prevent env leakage between the dev runner host and agent worktrees.

### Linked Worktree Reuse

Execution workspaces reuse existing linked worktrees when possible rather than creating new ones for every run. This avoids worktree proliferation and speeds up agent startup. Fix in this PR addressed cases where linked worktree detection failed, causing unnecessary worktree creation.

### Auto-Checkout on Scoped Issue Wake

When an issue-scoped agent wakes (via heartbeat), the harness automatically checks out the appropriate branch or worktree for that issue's context. This removes manual branch-switching from the agent's responsibilities and ensures agents start in the correct workspace state.

## Boundaries

- The dev runner manages worktree lifecycle (create, reuse, cleanup). It does not manage the git repository itself.
- Workspace import and configuration are separate from worktree management — the dev runner delegates to the workspace import system for project setup.
