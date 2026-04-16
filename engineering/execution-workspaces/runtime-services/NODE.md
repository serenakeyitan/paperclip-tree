---
title: "Workspace Runtime Services"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Workspace Runtime Services

How agents manage long-running services and one-shot jobs within execution workspaces, and the typed protocol that coordinates workspace operations between the server and adapters.

**Source:** `packages/shared/src/types/workspace-runtime.ts`, `packages/shared/src/workspace-commands.ts`, `server/src/services/execution-workspaces.ts`, `scripts/provision-worktree.sh`

## Key Decisions

### Typed Workspace Commands Protocol

Workspace operations are classified into two categories: **services** (long-running, supervised processes like dev servers or watchers) and **jobs** (one-shot operations like builds or migrations). The distinction is encoded in a shared type layer (`workspace-runtime.ts`) so both the server and adapters agree on lifecycle semantics — services are expected to run continuously and are monitored, while jobs run to completion and report a result.

### Runtime Service Lifecycle

Runtime services within a workspace are tracked in the `workspace_runtime_services` database table with formalized state transitions. The server manages service startup, health monitoring, and teardown as part of the workspace lifecycle. When a workspace is reclaimed or an agent stops, its associated services are cleaned up.

### Automated Worktree Provisioning

The `provision-worktree.sh` script standardizes worktree setup, replacing ad-hoc manual provisioning. It handles git worktree creation, dependency installation, and environment preparation in a single repeatable step. This is used by both the dev runner and production workspace creation paths.

## Boundaries

- This node covers the *runtime service protocol and lifecycle* within workspaces. The parent node (Execution Workspaces) covers worktree isolation, hardening, and workspace-to-issue linking.
- Adapter-specific service management (e.g., how Claude vs Codex handles dev servers) belongs in the respective adapter nodes.
