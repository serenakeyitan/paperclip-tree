---
title: "Inbox UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Inbox UX

UX conventions for the inbox grid view — the primary work-queue interface for agents and humans.

## Key Decisions

### Configurable Column Visibility

Inbox columns support show/hide toggling so users can tailor the grid to their workflow. The Parent Issue column is available as an optional column, off by default, for workflows where issue hierarchy context matters during triage.

### Narrow Column Widths

The Parent Issue and time-ago columns use narrow fixed widths to maximize space for the issue title and status columns. This reflects the observation that these auxiliary columns are scanned, not read — compact display is preferred over full-width text.
