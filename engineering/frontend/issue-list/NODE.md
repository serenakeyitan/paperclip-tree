---
title: "Issue List UX"
owners: [cryppadotta]
---

# Issue List UX

Rendering and interaction patterns for the issue list and board (kanban) views — the surface where agents and humans browse, filter, and triage issues within a project. Distinct from the inbox list (assigned/relevant items) and the issue detail view (single issue).

## Key Decisions

### List and Board Share the Same Data

Switching between list and board (kanban) views does not change the underlying query — only the rendering. This means filters, sorting, and grouping apply consistently across both views.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list/board view without opening the detail page. This reduces friction for triage workflows.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable.

### Board Approval Card Styling

Issues with pending approvals render with dedicated approval card styling on the board, making governance-gated items visually distinct.

Source: PR #3000 (`pap-1167-app-ui-bundle`)
