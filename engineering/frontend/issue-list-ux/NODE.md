---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend", "product/task-system"]
---

# Issue List UX

The issue list is the primary surface for browsing, filtering, grouping, and acting on issues within a project or company context. It supports both list-style and board (kanban) views over the same underlying data and API.

The issue list is a separate surface from the inbox list. While the inbox shows notifications and assigned tasks across all contexts, the issue list shows all issues within a specific project or scope. UX improvements to the issue list are scoped independently from inbox UX.

## Key Decisions

- **List and board are views over the same data and API.** Switching between list and board does not change the underlying query — only the rendering component.
- **Workspace grouping uses `resolveIssueFilterWorkspaceId`.** When a workspace is selected in the sidebar, the issue list filters to that workspace's issues. Switching workspaces updates the query parameter and React Query refetches without a page reload.
- **View state persisted in company-scoped local storage.** Filter, sort, and group-by state are stored in `localStorage` keyed by a `viewStateKey`, not in URL query params. This means view state is per-device, not shareable via URL.
- **Client-side filtering via `IssueFiltersPopover`.** Filters (status, assignee, priority, labels) are applied client-side on the already-loaded dataset rather than round-tripping to the server per filter change. This keeps the list responsive during rapid triage.
- **Page search blurs on Enter and conditionally on Escape.** The search input blurs on `Enter` (`shouldBlurPageSearchOnEnter`) to confirm the search. It blurs on `Escape` only when the field is empty (`shouldBlurPageSearchOnEscape`). This avoids losing the search field during refinement while still allowing keyboard dismissal.
- **List controls use icon-only buttons unconditionally.** Sort and group-by controls render as `<Button variant="outline" size="icon">` with a `title` attribute. The filter button (`IssueFiltersPopover`) keeps its default `buttonVariant="ghost"` but also receives `iconOnly` to render at `size="icon"`. The icon-only mode is always active, not viewport-gated.
- **Inline workflow transitions mutate optimistically.** Status changes, assignee changes, and priority changes made from the list view use React Query optimistic updates so the row reflects the new value immediately without waiting for the server round-trip.
- **Shared `IssueProperties` component.** The property panel used in the list is the same component rendered in the detail view, so agents and humans see consistent metadata presentation regardless of the entry point.

## Open Questions

- Saved/named views — whether users can save filter+sort combinations as named views.
- Bulk actions — selecting multiple issues for batch status/assignee changes.
