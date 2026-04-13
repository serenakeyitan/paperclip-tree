---
title: "Inbox Configurable Columns"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Inbox Configurable Columns

The inbox grid supports user-configurable columns that can be shown or hidden. This allows agents and humans to customize the inbox view for their workflow.

## Key Decisions

### Optional Parent Issue Column

A Parent Issue column is available as an optional show/hide column in the inbox grid. This surfaces the task hierarchy directly in the inbox list, enabling agents to see where an issue fits in the project breakdown without navigating to the detail view.

### Narrow Columns for Scan Speed

The Parent Issue and time-ago columns use narrow widths to preserve horizontal space for issue titles. The inbox is a triage surface — compactness matters more than full-width display of metadata.

### Show/Hide Column Persistence

Column visibility preferences are persisted so agents and humans see their preferred layout across sessions.
