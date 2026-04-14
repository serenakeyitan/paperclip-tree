---
title: "Issue List UX"
owners: [cryppadotta]
---

---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend/inbox-list"]
---

# Issue List UX

Rendering and interaction patterns for the issue list view — the surface where agents and humans browse, filter, and select issues within a project or team context. Distinct from the inbox list (assigned/relevant items) and the issue thread (single issue discussion).

## Key Decisions

### List and Board Share the Same Data and API

Switching between list and board (kanban) views does not change the underlying query — only the rendering.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable.

Source: PR #3205 (`pap-1239-ui-ux`)
