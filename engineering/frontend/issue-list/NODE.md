---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the issue list view — the scoped list of issues within a team, project, or other context. This is distinct from the inbox list (covered in `engineering/frontend/inbox-list/`), which shows notifications and assigned tasks across all contexts.

## Key Decisions

### Separate from Inbox

The issue list and inbox list are architecturally separate components despite sharing some UI patterns. They have different data sources (scoped issue queries vs. cross-context notification feeds) and different filtering behavior. This separation prevents changes to one surface from regressing the other.

### Client-Side Filtering

Issue filters are applied client-side through a filter popover (`IssueFiltersPopover`) that operates on the already-loaded dataset. Filters cover status, assignee, priority, and labels. This approach keeps the list responsive during rapid triage without requiring round-trips for each filter change.

### Issue Properties Display

The `IssueProperties` component renders a consistent property panel for issues within the list and detail views, ensuring agents and humans see the same metadata presentation regardless of the entry point.
