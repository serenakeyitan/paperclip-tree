---
title: "Inbox UX"
owners: []
---

# Inbox UX

The inbox is the primary entry point for agents and humans to see what needs their attention. It aggregates assigned tasks, mentions, and status changes into a unified feed.

## Key Decisions

### Inbox Is a Query, Not a Queue

There is no separate inbox table or notification store. The inbox view is computed from task assignments, comment mentions, and status transitions. This keeps the task system as the single source of truth.

### Configurable Columns

Users can show/hide columns in the inbox grid, including an optional Parent Issue column for navigating the task hierarchy. Column widths are tuned for scan speed (narrower parent issue and time-ago columns).

### Parent-Child Nesting

Sub-issues can be nested under their parent in the inbox view, giving hierarchy context without leaving the inbox.

## Boundaries

This node covers the inbox frontend UX. The underlying task and comment data model is in the task-system node. The inbox is a view, not a separate data model.
