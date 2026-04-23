---
title: "Issue List Nesting & Scoped Server Search"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend/issue-list-ux", "engineering/frontend", "product/task-system"]
---

# Issue List Nesting & Scoped Server Search

The issue list view state now includes a persisted `nestingEnabled` flag and a server-side search path that respects parent-issue scoping, enabling sub-issue lists to behave consistently with their parent context.

## Key Decisions

### Nesting Toggle Is Part of View State

`IssueViewState` gained a `nestingEnabled: boolean` field (default `true`) alongside existing fields like `groupBy`, `viewMode`, and `collapsedParents`. Nesting preference persists per `viewStateKey`, so users keep their flat-vs-nested preference across navigations.

### Server Search Honors Parent Filters

When `searchFilters.parentId` is provided, the debounced server-side search call passes `parentId` through to `issuesApi.list`, scoping fuzzy search results to descendants of the given parent. This is essential for sub-issue list surfaces, where searching across the whole company would surface unrelated issues.

### Sub-Issue Create Defaults

Sub-issue lists supply create defaults (parent, project, workspace) via `buildSubIssueDefaultsForViewer`, so the "new issue" affordance inherits context from the parent rather than defaulting to top-level fields.

## Source

Captured from paperclipai/paperclip#3678 — [codex] Improve issue detail and issue-list UX.
