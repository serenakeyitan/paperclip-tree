---
title: "Issue List & Board Workflows"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue List & Board Workflows

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Decisions

### List and Board Are Views Over the Same Data

Switching between list and board does not change the underlying query — only the rendering. Both views share the same filter, sort, and grouping state.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable. Filtering supports status, assignee, priority, team, and label dimensions.

## Open Questions

- Saved/named views — whether users can save filter+sort combinations as named views
- Bulk actions — selecting multiple issues for batch status/assignee changes
- Default sort order and how it differs between agent-facing and human-facing contexts
