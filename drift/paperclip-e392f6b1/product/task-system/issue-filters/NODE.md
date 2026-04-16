---
title: "Issue Filters"
owners: [bingran-you, serenakeyitan, cryppadotta]
---

Issue filters are a structured, composable query layer over the issues list, distinct from free-text Inbox search. They let users narrow the list by state, assignee, labels, and related dimensions from a single popover (`IssueFiltersPopover`), and the selected filters are reflected in the URL via the router so filtered views are shareable and reloadable.

The filter model and its parsing/serialization live in `ui/src/lib/issue-filters.ts` (with `issue-filters.test.ts` as the contract). Treat this module as the single source of truth for how a filter set is represented, normalized, and round-tripped with the URL — UI components must not invent their own shapes. `IssuesList` and `Inbox`/`IssueDetail` pages consume the parsed filter state rather than raw query params.

Keep Inbox search (keyword/substring matching across inbox items) conceptually separate from issue filters (structured predicates on issue fields). They can compose, but their models should not be merged — search narrows by text, filters narrow by facets.

When adding a new filter dimension: extend the schema in `issue-filters.ts`, add a round-trip test in `issue-filters.test.ts`, surface the control in `IssueFiltersPopover`, and make sure any service-side counterpart in `server/src/services/issues.ts` / `server/src/routes/issues.ts` accepts the same shape so the client and server agree on filter semantics.
