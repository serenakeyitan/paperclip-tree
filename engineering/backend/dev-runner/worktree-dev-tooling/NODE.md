---
title: "Worktree Dev Tooling"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Worktree Dev Tooling

Dev-time tooling that makes the Paperclip development experience work correctly inside git worktree workspaces. When agents run in isolated worktrees (see Execution Workspaces), standard dev tooling (Vite, pnpm workspace links, HTTP logging) can break because worktrees don't share the same node_modules or file watchers as the main checkout.

## Key Decisions

### Workspace Package Link Enforcement

A dedicated script (`scripts/ensure-workspace-package-links.ts`) ensures that pnpm workspace symlinks (`workspace:*` protocol) resolve correctly inside worktree directories. Worktrees created by git share the `.git` directory but not `node_modules`, so internal package links can break. This script is run as part of worktree setup to re-establish links.

### Vite File Watching in Worktrees

A `vite-watch` utility (`ui/src/lib/vite-watch.ts`) handles file-system watching quirks specific to worktree environments. Standard Vite file watchers may not detect changes correctly when the working directory is a worktree rather than the root checkout.

### HTTP Log Policy Middleware

An HTTP log policy middleware (`server/src/middleware/http-log-policy.ts`) filters request logging to reduce noise during dev runs inside worktrees, where high-frequency polling and workspace health checks generate excessive log output.

## Boundaries

- These are dev-time utilities, not production concerns. They are relevant only when running the Paperclip dev server inside worktree workspaces.
- Worktree creation and lifecycle management remain the responsibility of the Dev Runner and Execution Workspaces modules.
