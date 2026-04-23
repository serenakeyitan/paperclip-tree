---
title: "UI Storybook"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend"]
---

# UI Storybook

The board UI ships a Storybook for component review. Config, stories, and fixtures live under `ui/storybook/` so component review files stay out of the app source routes.

## Layout

- `ui/storybook/.storybook/` — Storybook config (`main.ts`, `preview.tsx`, Tailwind entry, styles). Uses `@storybook/react-vite` with `@storybook/addon-docs` and `@storybook/addon-a11y`.
- `ui/storybook/stories/` — Story files grouped by surface (foundations, navigation-layout, forms-editors, dialogs-modals, chat-comments, budget-finance, agent-management, issue-management, projects-goals-workspaces, control-plane-surfaces, status-language, data-viz-misc, ux-labs, overview).
- `ui/storybook/fixtures/paperclipData.ts` — Canonical fixture data (companies, agents, issues, approvals, projects, live runs, dashboard summary, sidebar badges, auth session) used by stories.
- `ui/storybook/.storybook/preview.tsx` installs a global fetch shim so stories render against fixtures instead of the real API, and wires up all app providers (Company, Theme, Sidebar, Toast, Dialog, Panel, Breadcrumb, EditorAutocomplete, QueryClient, MemoryRouter, Tooltip).

## Commands

Root scripts: `pnpm storybook` (dev server on port 6006) and `pnpm build-storybook` (static output to `ui/storybook-static/`). Both delegate to the `@paperclipai/ui` package.

## Why Storybook replaced the in-app UX labs

The legacy `/tests/ux/chat`, `/tests/ux/invites`, and `/tests/ux/runs` routes (`IssueChatUxLab`, `InviteUxLab`, `RunTranscriptUxLab`) were removed from `App.tsx`. Component review now happens in Storybook rather than as live app routes, keeping the production router free of review-only surfaces.

## Component changes to support Storybook

Several components were extended so stories can drive them deterministically: `CompanySwitcher`, `SidebarCompanyMenu`, and `SidebarAccountMenu` now accept optional controlled `open`/`onOpenChange` props; `BudgetSidebarMarker` gained a `level` prop (`healthy` | `warning` | `critical`); `BudgetIncidentCard` exposes an incident state badge; `MarkdownEditor` gained a `readOnly` mode; `KeyboardShortcutsCheatsheet` exports a `KeyboardShortcutsCheatsheetContent` piece for embedding in stories.
