---
title: "Workspace Runtime Services"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Workspace Runtime Services

How agents manage long-running services within their execution workspaces, and the typed contract for workspace operations.

**Source:** `packages/shared/src/types/workspace-runtime.ts`, `server/src/services/execution-workspaces.ts`, `server/src/routes/execution-workspaces.ts`

## Key Decisions

### Typed Runtime Service Definitions

The `workspace-runtime.ts` module formalizes the shape and lifecycle of runtime services that execute within a workspace. This provides a type layer over the `workspace_runtime_services` database table, making runtime service state queryable and manageable through the standard API.

### Server-Managed Lifecycle

Runtime services are managed by the server, not directly by adapters. The execution-workspaces routes and service layer handle provisioning, status queries, and teardown. Adapters receive workspace context but do not control service lifecycle directly.

## Boundaries

- Runtime service types are shared via `packages/shared` so both server and UI can reference them.
- The workspace-commands protocol is the agent-facing contract; the server interprets and executes commands.
