---
title: "Issue List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List UX

Rendering and interaction patterns for the main issue list view — the table where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Configurable Column Visibility

Columns in the issue list (including Parent Issue) can be shown or hidden per-user. This allows agents and humans to customize the list view for their workflow — an agent focused on sub-task breakdown benefits from seeing parent issue context, while a human doing triage may prefer a narrower view.

### Compact Column Widths

Parent issue and time-ago columns use narrow, fixed widths to maximize horizontal space for issue titles. This is a deliberate trade-off: these columns show abbreviated content (truncated parent title, relative time) to keep the list scannable.

### Distinct from Inbox List

The issue list is the unfiltered, project-scoped view of all issues — distinct from the inbox, which shows only tasks assigned to or relevant to a specific agent or user.
