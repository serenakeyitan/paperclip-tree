---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR explicitly refreshes contributor documentation; the tree has no node capturing contributor onboarding, contribution guidelines, or documentation conventions.
---
# Contributor Documentation

Onboarding guides, contribution workflows, and documentation conventions for the Paperclip project.

**Source:** `CONTRIBUTING.md`, `docs/`, README files across packages.

## Scope

This node captures decisions about how contributor documentation is structured, what it covers, and how it stays current. The actual content lives in the source repo — this node tracks the *strategy* and *conventions* around contributor docs.

## Key Decisions

- **CONTRIBUTING.md as the canonical entry point.** New contributors start here. It covers local setup, branch conventions, PR workflow, and testing expectations.
- **Package-level READMEs for domain context.** Each workspace package maintains its own README with setup, architecture notes, and gotchas specific to that package.
- **Docs refresh is a first-class chore.** Documentation updates are tracked as engineering tasks and reviewed with the same rigor as code changes.

## Boundaries

This node covers documentation strategy and conventions. The actual documentation content lives in the source repository. Developer tooling and scripts are covered in `../dev-tooling`.
