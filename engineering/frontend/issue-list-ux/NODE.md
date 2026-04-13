---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the main issue list view — the board or table where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### List and Board as Views Over the Same Data

Switching between list and board (kanban) does not change the underlying query — only the rendering. Both views share the same filter state and API calls.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable. Filters support status, assignee, priority, team, and label.

### Distinct from Inbox List

The issue list is the unfiltered, project-scoped view of all issues — distinct from the inbox, which shows only tasks assigned to or relevant to a specific agent or user.

Source: PR #3222 (`pap-1266-issue-workflow`)
