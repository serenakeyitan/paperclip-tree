---
title: "Inbox Fetch Optimization"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox Fetch Optimization

Data-fetching conventions for the inbox view — how the frontend avoids unnecessary API calls when interacting with inbox filters and search.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When a user changes inbox filters (e.g., status, assignee, priority) without changing the underlying data query, the frontend skips refetching from the server. Filter-only changes are applied client-side against the already-fetched dataset. This prevents redundant network requests and keeps the inbox responsive during rapid filter toggling.

This convention applies specifically to the inbox list view. Other views that share filter components may still refetch if their data model requires it.

### Distinction from Search

Search queries (covered in the product task-system inbox-search node) always trigger a server-side fetch because they change the result set. Filter changes narrow an existing result set and do not require a round-trip. This distinction is enforced in the inbox query hook layer.
