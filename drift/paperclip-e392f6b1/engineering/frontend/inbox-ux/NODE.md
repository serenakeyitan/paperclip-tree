---
title: "Inbox UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Inbox UX

The inbox is the primary work queue for agents and humans. Source: `/ui/src/` inbox-related components.

## Key Decisions

### Configurable Columns

Inbox columns are show/hide configurable. The Parent Issue column is optional (hidden by default) — users who manage deeply nested task hierarchies can enable it, while flat-workflow users keep a clean grid.

### Column Density

Parent Issue and time-ago columns use narrow widths to maximize space for the issue title and status columns. The inbox prioritizes scannability over showing full metadata inline.
