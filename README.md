---
title: "Paperclip Context Tree"
owners: [bingran-you, serenakeyitan]
---

# Paperclip Context Tree

This repository is the dedicated **Context Tree** for Paperclip.

It is the living source of truth for **decisions, constraints, ownership, and
cross-domain relationships** around the Paperclip project. It is **not** the
implementation repo and it is **not** the place for low-level execution detail.

**Source repository:** [paperclipai/paperclip](https://github.com/paperclipai/paperclip)

## What Lives Here

- Domain maps and boundaries
- Architectural and product decisions
- Ownership and review responsibility
- Cross-domain links that future agents and humans need in order to decide well

## What Does Not Live Here

- Function signatures and internal APIs
- Database schemas and migrations
- UI component implementation details
- Adapter parser code or workflow YAML internals

Those belong in the source repository. This tree captures the **why** and the
**how domains connect**.

## Start Here

1. Read [NODE.md](NODE.md) for the top-level map.
2. Open the domain `NODE.md` that matches your task.
3. Read the leaf decision records linked from that domain.
4. Follow any related-domain links when the decision crosses boundaries.

If you are working as an agent, also read:

- [AGENTS.md](AGENTS.md)
- [CLAUDE.md](CLAUDE.md)

## Current Maturity

This tree is currently in a **first-pass / baseline coverage** state:

- Root and major domains are established
- Member ownership is defined
- Validation and CODEOWNERS generation are in place
- Initial decision records exist for the most important product, engineering,
  infrastructure, plugin, and adapter concerns

The tree is usable today, but it is still being deepened. Some domains have
rich `NODE.md` summaries but fewer leaf decision records than they should in a
mature state.

## Domain Map

- [engineering/](engineering/NODE.md) — backend, frontend, database, shared
  contracts, and CLI
- [adapters/](adapters/NODE.md) — runtime integrations for Codex, Claude,
  Cursor, Gemini, OpenClaw, OpenCode, and Pi
- [plugins/](plugins/NODE.md) — plugin SDK, runtime boundary, and examples
- [product/](product/NODE.md) — company model, agent model, task system, and
  governance
- [infrastructure/](infrastructure/NODE.md) — deployment, CI/CD, and testing
- [members/](members/NODE.md) — owners and contributors

## File Conventions

- Every domain directory contains a `NODE.md`.
- `NODE.md` explains the domain's purpose, boundaries, and major decisions.
- Leaf markdown files under a domain capture a specific decision in a stable,
  linkable form.
- `owners` in frontmatter define who approves changes.
- `soft_links` connect related domains without turning the tree into a full
  graph database.

## Common Commands

```bash
npx -p first-tree first-tree inspect --json
npx -p first-tree first-tree verify
npx -p first-tree first-tree generate-codeowners
npx -p first-tree first-tree inject-context
```

## Working Rule

Decide in the tree, execute in the source repo.

If a task changes a decision, constraint, ownership boundary, or cross-domain
relationship, update this tree. If a task changes only implementation detail,
update the source repo instead.
