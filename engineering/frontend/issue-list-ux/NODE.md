---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend", "product/task-system"]
---

# Issue List UX

The issue list is the primary surface for browsing, filtering, grouping, and acting on issues within a project or company context. It supports both list-style and board (kanban) views over the same underlying data and API.

The issue list is a separate surface from the inbox list. While the inbox shows notifications and assigned tasks across all contexts, the issue list shows all issues within a specific project or scope. UX improvements to the issue list are scoped independently from inbox UX.

## Key Decisions

- **Workspace grouping uses the `workspaceId` query parameter.** When a workspace is selected in the sidebar, the issue list filters to that workspace's issues by passing `workspaceId` on the backend query. Switching workspaces does not reload the page — the query parameter updates and React Query refetches.
- **Page-level search blurs on Escape and on outside-click.** The search input in the issue list header clears its active/focused state on `Escape` keydown and on `mousedown` outside the search container. It does not blur on `Enter` — `Enter` confirms the search and keeps focus so the user can refine. This avoids the pattern of losing the search field after submitting.
- **List controls use icon-only buttons in compact mode.** Filter, sort, and group-by controls in the list toolbar render as icon-only `<Button variant="ghost" size="icon">` when the viewport width is below the `md` breakpoint (768 px). Labels are hidden via `hidden md:inline` and a `Tooltip` is attached to each icon button so the action name is discoverable on hover.
- **List and board are views over the same data and API.** Switching between list and board does not change the underlying query — only the rendering component.
- **Filters are URL-driven.** Filter state (`status`, `assignee`, `priority`, `label`, `workspaceId`) is encoded in the URL query string so views are shareable and bookmarkable. The frontend reads filter state from `useSearchParams` and writes it back on change.
- **Inline workflow transitions mutate optimistically.** Status changes, assignee changes, and priority changes made from the list view use React Query optimistic updates so the row reflects the new value immediately without waiting for the server round-trip.

## Open Questions

- Saved/named views — whether users can save filter+sort combinations as named views.
- Bulk actions — selecting multiple issues for batch status/assignee changes.
