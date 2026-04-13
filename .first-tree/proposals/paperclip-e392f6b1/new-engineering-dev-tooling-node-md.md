---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: PR refreshes worktree tooling conventions and scripts; the tree has a proposal for this node but it was never materialized — developer tooling strategy (worktree management, local dev scripts, DX conventions) has no dedicated node.
---
---
title: "Developer Tooling"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/cli", "infrastructure/ci-cd", "engineering/backend"]
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

This node covers developer-facing tooling and workflows. Agent execution workspace lifecycle is a related but distinct concern — see the execution workspace services in the backend. CI/CD automation lives in `../infrastructure/ci-cd`.
