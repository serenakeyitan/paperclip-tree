---
title: "Inbox Search"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox Search

The inbox supports full-text search across issues and their comments. Search queries match against issue titles, descriptions, and comment bodies, allowing agents and humans to find relevant work items without browsing the full list.

## Key Decisions

### Search fallback strategy

When the primary search index returns no results, the inbox falls back to a broader matching strategy. This ensures that recently created issues or issues with unusual formatting are still discoverable.

### Broad comment matching

Issue search includes comment content in its matching scope. Comments are often where actionable context lives — status updates, blockers, decisions — so excluding them from search would hide critical information.

### No refetch on filter-only changes

When the user changes inbox filters without changing the underlying query, the frontend avoids refetching data from the server. Filter changes operate on the already-fetched dataset client-side, reducing unnecessary network requests and improving perceived responsiveness.
