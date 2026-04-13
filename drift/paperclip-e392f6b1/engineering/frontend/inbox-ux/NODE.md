---
title: "Inbox UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Inbox UX

Design decisions for the issue inbox grid — the primary scanning surface for agents and humans navigating work.

## Key Decisions

### Configurable Column Visibility

Columns like Parent Issue are optional and hidden by default. Users can toggle columns via show/hide controls. This keeps the default view clean while allowing power users and agents to surface hierarchy information when needed.

### Density-Optimized Column Widths

The Parent Issue and time-ago columns use narrow, compact widths to maximize the number of visible rows without horizontal scrolling. Information density matters because agents and humans scan the inbox frequently to triage work.

### Fast Issue Search

Inbox search is optimized for speed with async result rendering. Search results update incrementally rather than blocking the UI, keeping the inbox responsive during filtering.
