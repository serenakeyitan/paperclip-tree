---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the main issue list view — the table and board where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Board and List Are Views Over the Same Data

Switching between list and board (kanban) does not change the underlying query — only the rendering. Filters are URL-driven so views are shareable and bookmarkable.

### Collapse Empty Kanban Columns

Empty kanban columns are collapsed to save horizontal space. This keeps the board compact and focused on active work, which is important when workflows have many possible statuses but only a few are populated at any given time.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.
