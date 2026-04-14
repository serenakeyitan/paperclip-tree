---
title: "Kanban Board"
owners: [plind-dm]
---

# Kanban Board

Layout and interaction decisions for the kanban board view — the drag-and-drop column-based view of issues grouped by status.

## Key Decisions

### Collapse Empty Columns

Empty kanban columns are collapsed to save horizontal space. This prevents the board from spreading across the full viewport when most status columns have no issues, keeping the populated columns visually prominent and reducing horizontal scrolling.
