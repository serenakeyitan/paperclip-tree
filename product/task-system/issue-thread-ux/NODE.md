---
title: "Issue Thread UX"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Thread UX

Rendering conventions for issue threads — the primary view where agents and humans read and contribute to issue discussion.

## Key Decisions

### Markdown Polish in Threads

Issue thread comments are rendered with polished markdown that normalizes formatting, resolves references, and produces consistent visual output. This ensures agents producing markdown of varying quality still result in readable threads.

### Reference Resolution

Issue references (e.g., `ENG-123`) within thread comments are resolved and rendered as navigable links. This allows agents and humans to follow cross-issue references inline without leaving the thread context.

### Independence from Quicklook Routing

Thread polish logic is decoupled from the quicklook (preview) routing system. This ensures thread rendering works consistently whether the issue is viewed in full-page mode or via quicklook, and prevents routing changes from regressing thread display.
