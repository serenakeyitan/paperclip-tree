---
title: "Dev Environment Tooling"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Dev Environment Tooling

Dev-time tooling that improves the Paperclip development experience, covering asset routing, file-watch filtering, and workspace package link repair. These changes apply broadly to the dev server, not only to worktree-based workflows.

## Key Decisions

### Dev Asset Routing Order

In Vite dev mode, the server serves public assets (from `ui/public/`) via `express.static` **before** the HTML shell catch-all handler (`server/src/app.ts`). This ensures that static files like `/sw.js`, `/favicon.ico`, and `/site.webmanifest` are served directly rather than falling through to the Vite HTML transform. The set of known static paths is maintained in `VITE_DEV_STATIC_PATHS`.

### Vite Dev Watch Filtering

A `vite-watch` utility (`ui/src/lib/vite-watch.ts`) reduces unnecessary dev-server work by ignoring test files (`*.test.*`, `*.spec.*`) and test directories (`__tests__/`, `tests/`) during file watching. It also provides a WSL polling fallback for `/mnt/` paths where `inotify` is unavailable. The filtering is general-purpose dev ergonomics, not worktree-specific.

### Workspace Package Link Repair

A script (`scripts/ensure-workspace-package-links.ts`) detects and repairs stale pnpm workspace symlinks (`workspace:*` protocol). The script now runs whenever a workspace-level `node_modules` directory exists, not only inside linked worktree checkouts. This broadens the repair to cover any checkout where workspace links may have gone stale (e.g., after a fresh `pnpm install` or branch switch).

## Boundaries

- These are dev-time utilities, not production concerns. They affect only the local development server and build tooling.
- Worktree creation and lifecycle management remain the responsibility of the Dev Runner and Execution Workspaces modules.
