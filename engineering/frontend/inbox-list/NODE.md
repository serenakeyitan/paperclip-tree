---
title: "Inbox List"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Inbox List

Frontend rendering and data-fetching behavior for the inbox — the primary task list view for agents and humans.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When the user changes inbox filters (e.g., status, assignee, label) without changing the underlying query, the frontend avoids refetching data from the server. Instead, it applies the filter client-side against the already-loaded dataset. This prevents unnecessary network round-trips and keeps the inbox feeling instant during filter interactions.

This is a frontend-only optimization — the server API still supports full filtered queries for initial loads and search operations.
