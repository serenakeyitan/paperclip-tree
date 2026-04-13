---
title: "Inbox Configurable Columns"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Inbox Configurable Columns

The inbox list supports user-configurable column visibility — users and agents can show or hide optional columns to customize their task view.

## Key Decisions

### Optional Columns with Show/Hide Toggle

Columns beyond the core set (title, status, assignee) are optional and toggled via a show/hide control. The Parent Issue column was added as the first optional column. This pattern allows the inbox to stay compact by default while giving power users access to additional context.

### Narrow Supplementary Columns

Supplementary columns (Parent Issue, time-ago) use narrower widths than primary columns. This prevents the inbox grid from becoming horizontally crowded when multiple optional columns are enabled, keeping the title column — the most important for scanning — as wide as possible.
