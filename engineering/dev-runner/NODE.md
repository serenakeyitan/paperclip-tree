---
title: "Dev Runner"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Dev Runner

The dev runner is the subsystem responsible for bootstrapping and managing execution workspaces for agent task runs during local development.

## Key Decisions

### Worktree-Based Isolation

Each execution workspace uses a git worktree to isolate agent work from the main working tree. The dev runner handles worktree creation, environment bootstrapping, and cleanup. Environment variables are scoped per-worktree to prevent cross-contamination between concurrent agent runs.

### Linked Worktree Reuse

When an agent resumes work on the same branch or issue, the dev runner reuses the existing linked worktree rather than creating a new one. This avoids workspace proliferation and preserves uncommitted agent state across session boundaries.

### Workspace Import

The dev runner imports workspace configuration (dependencies, environment, tooling) into each worktree during bootstrap. This import step was hardened to prevent regressions where missing or stale imports caused agent failures in fresh worktrees.
