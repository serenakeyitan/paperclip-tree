---
title: "Inbox UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Inbox UX

Layout and column conventions for the inbox grid — the triage surface where humans review and prioritize incoming work.

## Key Decisions

### Configurable Column Visibility

Inbox columns support show/hide toggling. The Parent Issue column is available as an optional column, off by default. This lets users customize the inbox density to their workflow — some users need parent context for triage, others don't.

### Compact Column Widths

The Parent Issue and time-ago columns use narrowed widths to maximize space for issue titles and status information. The inbox prioritizes scannability over full-detail display — detail belongs in the issue detail view.
