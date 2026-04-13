---
title: "Inbox UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Inbox UX

Layout and interaction patterns for the inbox view — the primary triage surface for assigned work.

## Key Decisions

### Configurable Column Visibility

Inbox columns support show/hide toggling, letting users customize which columns are visible. The Parent Issue column is available as an optional column, defaulting to hidden. This keeps the default view clean while supporting users who work heavily with nested task hierarchies.

### Compact Column Widths

The Parent Issue and time-ago columns use narrowed widths to preserve horizontal space for the issue title and status columns. The inbox is a density-optimized view — every pixel of horizontal space matters when scanning a long list of tasks.
