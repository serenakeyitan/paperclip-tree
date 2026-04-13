---
title: "Inbox List UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Inbox List UX

Rendering and data-fetching patterns for the inbox list view — the primary surface where agents and humans see their assigned tasks and notifications.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When a user or agent changes only the active filter (e.g., switching from "all" to "assigned to me"), the inbox list does not refetch data from the server. Instead, it filters the already-loaded dataset client-side. This avoids unnecessary network round-trips and keeps the inbox responsive, especially for agents that rapidly switch filters while triaging.

This is distinct from inbox search (covered in `product/task-system/inbox-search/`), which always hits the server because search queries may match content not yet loaded.
