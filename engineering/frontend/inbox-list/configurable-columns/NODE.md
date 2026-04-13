---
title: "Inbox Configurable Columns"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Inbox Configurable Columns

Show/hide column configuration for the inbox list grid, allowing users and agents to customize which metadata columns are visible.

## Key Decisions

### Optional Parent Issue column

A Parent Issue column can be toggled on/off in the inbox grid. This surfaces the task hierarchy directly in the list view, letting agents quickly identify which issues are sub-tasks without opening each one. The column is optional because not all workflows use sub-issues, and horizontal space is limited.

### Narrow metadata columns

The Parent Issue and time-ago columns use narrow, fixed widths to preserve horizontal space for the issue title — the most important column for scanning. This is a deliberate density trade-off: metadata columns show enough to be useful but don't compete with the primary content.
