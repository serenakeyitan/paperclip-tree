---
title: "Issue List Workflows"
owners: []
---

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style and list-style views.

## Key Patterns

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### Filtering and Grouping

Issue lists support filtering by status, assignee, priority, team, and label. Grouping (e.g., by status column or by assignee) provides different lenses on the same data. These filters map directly to query parameters on the backend issues API.

## Key Decisions

- **List and board are views over the same data and API.** Switching between list and board does not change the underlying query — only the rendering.
- **Filters are URL-driven.** Filter state is encoded in the URL so that views are shareable and bookmarkable.

## Open Questions

- Saved/named views — whether users can save filter+sort combinations as named views
- Bulk actions — selecting multiple issues for batch status/assignee changes
- Default sort order and how it differs between agent-facing and human-facing contexts
