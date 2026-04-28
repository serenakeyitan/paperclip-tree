---
title: "Workspaces Navigation & Experimental Gating"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/execution-workspaces/NODE.md, engineering/frontend/NODE.md, product/task-system/NODE.md]
---

# Workspaces Navigation & Experimental Gating

The UI exposes a top-level **Workspaces** surface at `/workspaces` (page component `ui/src/pages/Workspaces.tsx`) that lists project workspaces across the company, separate from the per-project `projects/:projectId/workspaces` view.

## Key Decisions

### Experimental Sidebar Gating

The sidebar link to `/workspaces` is rendered only when the instance experimental setting `enableIsolatedWorkspaces` is `true`. `Sidebar.tsx` reads this via `instanceSettingsApi.getExperimental()` and conditionally renders the nav item. The route itself is always registered in `App.tsx` (and added to `BOARD_ROUTE_ROOTS` in `company-routes.ts` so company-prefixed URLs redirect correctly) — only the sidebar entry point is gated. This lets deep links keep working for users who navigate directly while keeping the entry hidden by default.

### Shared Content Component

The per-project workspaces tab and the new global Workspaces page both render through `ProjectWorkspacesContent`, a shared component that owns the runtime control mutations and the close-workspace dialog. New surfaces that list project workspaces should reuse this component rather than re-implementing the runtime control wiring.
