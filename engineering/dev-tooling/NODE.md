---
title: "Developer Tooling"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Developer Tooling

Developer experience tooling, local development workflows, and worktree management for the Paperclip monorepo.

**Source:** `scripts/`, CLI `worktree` commands, `CONTRIBUTING.md`, development documentation.

---

## Worktree Tooling

Paperclip uses git worktrees as the primary isolation mechanism for concurrent agent and human development. The CLI provides worktree management commands that handle creation, linking to agent sessions, and cleanup. Worktree tooling bridges the gap between the task system's logical assignment and the physical filesystem workspace where agents execute.

## Key Decisions

- **Worktree-first development model.** Both human developers and AI agents use worktrees for isolated work. This unifies the development model rather than having separate workflows for human and agent contributors.
- **CLI as the primary dev tool surface.** Developer tooling is exposed through the `paperclipai` CLI rather than separate scripts, keeping the tool surface unified and discoverable.
- **Contributor documentation lives in-repo.** Onboarding guides, setup instructions, and contribution workflows are maintained alongside the code they describe, not in external wikis.

## Boundaries

This node covers developer-facing tooling and workflows. Agent execution workspace lifecycle is a related but distinct concern — see `engineering/execution-workspaces`. CI/CD automation lives in `infrastructure/ci-cd`.
