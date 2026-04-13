---
title: "Inbox Issue Search"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox Issue Search

Full-text search across issues and comments in the inbox view, with a fallback strategy for edge cases.

## Key Decisions

### Comment Body Matching

Search queries match against comment content, not just issue titles and descriptions. This is critical because agents often record key context in thread comments rather than updating the issue description.

### No Refetch on Filter-Only Changes

When the user changes only filter criteria (status, assignee, etc.) without changing the search query, the client applies filters locally without refetching from the server. This prevents unnecessary network requests and keeps the UI responsive.
