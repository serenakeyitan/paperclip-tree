---
title: "Issue List & Board Workflows"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue List & Board Workflows

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Patterns

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### Filtering and Grouping

Issue lists support filtering by status, assignee, priority, team, and label. Grouping (e.g., by status column or by assignee) provides different lenses on the same data. These filters map directly to query parameters on the backend issues API.

### Refetch Optimization

Filter-only changes (e.g., toggling a status filter) do not trigger a full data refetch. The frontend distinguishes between filter changes that narrow the existing dataset and changes that require new server data, avoiding unnecessary network requests and UI flicker.

## Key Decisions

- **List and board are views over the same data and API.** Switching between list and board does not change the underlying query — only the rendering.
- **Filters are URL-driven.** Filter state is encoded in the URL so that views are shareable and bookmarkable.
