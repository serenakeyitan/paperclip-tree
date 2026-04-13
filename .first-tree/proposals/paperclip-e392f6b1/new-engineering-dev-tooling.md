---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR refreshes worktree tooling and contributor docs; no engineering/dev-tooling node exists to capture developer tooling strategy, worktree management workflows, or DX conventions.
---
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
