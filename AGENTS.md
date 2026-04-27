<!-- BEGIN CONTEXT-TREE FRAMEWORK — do not edit this section -->
# Agent Instructions for Context Tree

You are working in a **Context Tree** — the living source of truth for decisions across the organization. Read and follow this before doing anything.

## Principles

1. **Source of truth for decisions, not execution.** The tree captures the *what* and *why* — strategic choices, cross-domain relationships, constraints. Execution details stay in source systems. If an agent needs this information to *decide* on an approach, it belongs in the tree. If the agent only needs it to *execute*, it stays in the source system.

2. **Agents are first-class participants.** The tree is designed to be navigated and updated by agents, not just humans. Domains are organized by concern — what an agent needs to know to act — not by repo, team, or org chart.

3. **Transparency by default.** All information is readable by everyone. Writing requires owner approval; reading is open.

4. **Git-native tree structure.** Each node is a file; each domain is a directory. Soft links allow cross-references without the complexity of a full graph. History, ownership, and review follow Git conventions.

See `.agents/skills/first-tree/references/principles.md` for detailed explanations and examples.

## Before Every Task

1. **Prefer the local checkout recorded by the source/workspace repo.** If this tree is associated with a source/workspace repo, read that repo's managed `FIRST-TREE-SOURCE-INTEGRATION:` section and `.first-tree/local-tree.json` first. If the recorded checkout exists locally, update it and use that copy for routine work.
2. **Use a temporary workspace clone only when the recorded checkout is missing.** If the tree has already been published but the recorded checkout is gone, clone a temporary copy into `.first-tree/tmp/<tree-repo-name>/` inside the active source/workspace repo, use it for the current task, and delete it before you finish.
3. **Use the sibling bootstrap checkout only before publish.** If this tree has not been published yet, it is okay to work from the sibling `*-tree` or legacy `*-context` bootstrap repo until publish records the GitHub URL and local checkout config.
4. **Read the root NODE.md** to understand the domain map.
5. **Read the NODE.md of every domain relevant to your task.** If unsure which domains are relevant, start from root and follow the structure — it's organized by concern, not by repo.
6. **Follow soft_links.** If a node declares `soft_links` in its frontmatter, read those linked nodes too. They exist because the domains are related.
7. **Read leaf nodes that match your task.** NODE.md tells you what exists in each domain — scan it and read the leaves that are relevant.

Do not skip this. The tree is already a compression of expensive knowledge — cross-domain relationships, strategic decisions, constraints. An agent that skips the tree will produce decisions that conflict with existing ones.

## During the Task

- **Decide in the tree, execute in source systems.** If the task involves a decision (not just a bug fix), draft or update the relevant tree node before executing.
- **The tree is not for execution details.** Function signatures, DB schemas, API endpoints, ad copy — those live in source systems. The tree captures the *why* and *how things connect*.
- **Bring source repos in as additional working directories.** When you need to inspect or edit code, open the relevant source repositories alongside this tree instead of copying execution detail into the tree.
- **Respect ownership.** Each node declares owners in its frontmatter. If your changes touch a domain you don't own, flag it — the owner needs to review.

## After Every Task

Ask yourself: **Does the tree need updating?**

- Did you discover something the tree didn't capture? (A cross-domain dependency, a new constraint, a decision that future agents would need.)
- Did you find the tree was wrong or outdated? That's a tree bug — fix it.
- Not every task changes the tree, but the question must always be asked.
- If the task changed decisions, constraints, rationale, or ownership, open the tree PR first. Then open the source/workspace code PR.
- If the task changed only implementation details, skip the tree PR and open only the source/workspace code PR.

## Reference

For ownership rules, tree structure, and key files, see [NODE.md](NODE.md) and `.agents/skills/first-tree/references/ownership-and-naming.md`.
<!-- END CONTEXT-TREE FRAMEWORK -->

<!-- BEGIN FIRST-TREE-REPO-INDEX-GUIDE -->
## Source Repo Index

- If `source-repos.md` exists in the tree root, use it as the quickest index of bound source/workspace repos and their GitHub URLs.
- The canonical machine-readable source of truth remains `.first-tree/bindings/`.
- When you need current code, use that repo index to open the relevant source repo as an additional working directory and refresh it locally.
<!-- END FIRST-TREE-REPO-INDEX-GUIDE -->

# Project-Specific Instructions

<!-- Add your project-specific agent instructions below this line. -->

## Paperclip Context

Paperclip is an open-source orchestration and control plane for autonomous AI companies. It coordinates multiple AI agents into company-like structures with task management, org charts, governance, budgets, and approval workflows.

**Repository:** [paperclipai/paperclip](https://github.com/paperclipai/paperclip)

### Key Architectural Decisions

1. **Company-scoped isolation** — All entities (agents, tasks, goals, budgets) are scoped to a `company_id`. Never leak data across companies.
2. **Adapter abstraction** — Agent runtimes (Claude, Codex, Cursor, Gemini, etc.) connect through a common adapter interface. Adapters handle lifecycle, heartbeats, and workspace management.
3. **Governance-first** — Budget hard-stops, approval gates, and full audit trails are core, not optional. Every mutation is logged with actor, action, and before/after state.
4. **Plugin extensibility** — Runtime skill injection via a plugin SDK. Plugins can add tools, scheduled jobs, and custom workflows.
5. **Monorepo with pnpm workspaces** — Server, UI, DB, adapters, plugins, CLI, and shared packages are all in one repo with clear boundaries.

### Domain Map

- **engineering/** — Backend (Express), frontend (React 19), database (Drizzle/Postgres), shared types, CLI
- **adapters/** — Agent runtime adapters (claude-local, codex-local, cursor-local, gemini-local, openclaw-gateway, opencode-local, pi-local)
- **plugins/** — Plugin SDK, runtime sandbox, and examples
- **product/** — Product strategy, company/agent/task model, governance decisions
- **infrastructure/** — Docker deployment, CI/CD, testing strategy
- **members/** — Team members and their responsibilities

## GitHub Task Workflow

- For `serenakeyitan/paperclip-tree` issues that ask for Breeze status propagation, update the issue label before responding and keep exactly one current status label: use `breeze:wip` while working, `breeze:human` if blocked on input, and `breeze:done` when finished.
