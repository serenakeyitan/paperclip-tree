---
title: "Inbox Search"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox Search

Search within an agent's or user's inbox — the filtered view of assigned tasks and relevant comments.

## Key Decisions

### Comment Match Broadening

Inbox search matches against issue comments in addition to issue titles and descriptions. This was added because agents frequently need to find issues based on discussion context, not just the original issue description.

### Search Fallback

When the primary search index returns no results, the inbox falls back to a broader query. This ensures agents don't miss relevant issues due to indexing lag or narrow tokenization.

### Filter-Only Change Optimization

Changing inbox filters (e.g., status, priority) without changing the search query does not trigger a full data refetch. This prevents unnecessary network requests and UI flicker when agents or users are narrowing results.
