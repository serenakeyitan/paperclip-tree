---
title: "Recent Selection Ordering"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/NODE.md, engineering/frontend/inbox-list/NODE.md, engineering/frontend/issue-list-ux/NODE.md]
---

# Recent Selection Ordering

Inline entity selectors (assignee, project, and similar single-value pickers) order their options by a shared rule: the currently-selected option first, then up to a small number of recently-used options, then the remaining options in their default order. The shared helper `orderItemsBySelectedAndRecent` in `ui/src/lib/recent-selections.ts` implements the rule, and `InlineEntitySelector` accepts a `recentOptionIds` prop so callers can opt in.

## Key Decisions

### Shared Storage Helpers Per Selection Domain

Recent selections are persisted in `localStorage` under per-domain keys (`paperclip:recent-assignees`, `paperclip:recent-projects`, …) via shared `readRecentSelectionIds` / `trackRecentSelectionId` primitives. Domain-specific modules (`recent-assignees.ts`, `recent-projects.ts`) wrap those primitives so each surface only depends on its own typed API. Assignees additionally namespace ids as `agent:<id>` vs `user:<id>` so agent and user recents can coexist in one list.

### Selected Option Always Pinned First

The currently-selected value (including the empty/no-value option when it is selected) is always rendered first, so users see the active state at the top of the popover regardless of recency. Recent options follow, capped by `RECENT_SELECTION_DISPLAY_LIMIT`, and the rest of the list keeps its caller-provided order.

### Track on Commit, Not on Hover

Recent ids are written only when a selection is actually committed (e.g. `trackRecentProject` is called from `onChange` handlers in `NewIssueDialog` and `RoutineRunVariablesDialog`, and assignee tracking happens on assignee change). Browsing the popover does not pollute the recents list.

### Opt-In Per Selector

`InlineEntitySelector` only reorders when the caller passes `recentOptionIds`; selectors that should keep a strict natural order (e.g. status, priority enums) are unaffected. This keeps the convention scoped to entity pickers where recency is meaningful.
