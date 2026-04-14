---
title: "Issue List UX"
owners: [cryppadotta]
---

# Issue List UX

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Decisions

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### Filtering and Grouping

Issue lists support filtering by status, assignee, priority, team, and label. Grouping (e.g., by status column or by assignee) provides different lenses on the same data.

### List and Board Are Views Over the Same Data

Switching between list and board does not change the underlying query — only the rendering.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable.

## Open Questions

- Saved/named views — whether users can save filter+sort combinations as named views
- Bulk actions — selecting multiple issues for batch status/assignee changes

Source: PR #3222 (`pap-1266-issue-workflow`)
