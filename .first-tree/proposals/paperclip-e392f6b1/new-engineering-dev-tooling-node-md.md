---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No existing node covers contributor documentation or developer-facing worktree tooling — dev-runner and execution-workspaces are about agent runtime worktrees, not contributor DX.
---
# Dev Tooling & Contributor Docs

Developer-facing tooling, scripts, and documentation for contributing to Paperclip. Covers worktree management utilities for local development, contributor onboarding guides, and development workflow conventions.

## Key Decisions

### Worktree Tooling for Contributors

Paperclip provides CLI and script-level tooling for managing git worktrees during local development. This is distinct from the server-side execution workspace system (which manages worktrees for AI agents at runtime). Contributor worktree tooling lives in the CLI and dev scripts.

### Contributor Documentation

Contributor docs (setup guides, development workflow, contribution guidelines) are maintained in the repo root and refreshed alongside tooling changes to stay in sync.

## Boundaries

- Agent runtime worktree management → `engineering/backend/dev-runner` and `engineering/execution-workspaces`
- CLI worktree commands → `engineering/cli`
- CI/CD and deployment → `infrastructure/ci-cd`
