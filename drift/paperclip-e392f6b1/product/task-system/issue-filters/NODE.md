---
title: "Issue Filters"
owners: [bingran-you, serenakeyitan, cryppadotta]
---

Issue filters are a structured, composable query layer over the issues list, distinct from free-text Inbox search. They let users narrow the list by state, priority, assignee, creator, labels, projects, and workspaces from a single popover (`IssueFiltersPopover`), plus an "hide routine executions" toggle when that capability is enabled.

The filter model and its parsing/normalization live in `ui/src/lib/issue-filters.ts` (with `issue-filters.test.ts` as the contract). Treat this module as the single source of truth for how a filter set is represented and normalized — UI components must not invent their own shapes. `IssuesList` reads the normalized filter state through this module when constructing its view.

Filter state persistence is local, not URL-based:

- `IssuesList` persists its full view state — selected filters, quick-filter preset, column layout — to `localStorage` keyed per list instance, and rehydrates through `normalizeIssueFilterState` so legacy blobs are safe on read.
- The `Issues` page only round-trips the free-text `q` search param (and an optional `participantAgentId`) in the URL. Structured filter selections are not part of the URL and are not shareable via link today.
- `Inbox` keeps its own preference blob in `localStorage` (`ui/src/lib/inbox.ts`), separate from issue-list filter state.

Keep Inbox search (keyword/substring matching across inbox items) conceptually separate from issue filters (structured predicates on issue fields). They can compose, but their models should not be merged — search narrows by text, filters narrow by facets.

When adding a new filter dimension: extend the schema in `issue-filters.ts`, add a round-trip test in `issue-filters.test.ts`, surface the control in `IssueFiltersPopover`, and make sure any service-side counterpart in `server/src/services/issues.ts` / `server/src/routes/issues.ts` accepts the same shape so the client and server agree on filter semantics.
