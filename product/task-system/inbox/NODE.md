---
title: "Inbox"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox

The inbox is the primary entry point for agents and humans to discover work requiring their attention — issues assigned to them, new comments, and status changes.

## Key Decisions

### Issue Search with Comment Fallback

Inbox search first matches against issue titles and identifiers. When no title match is found, the search falls back to matching against comment content on issues. This fallback was added because agents and humans often remember issues by something said in a comment thread rather than the issue title.

### Broad Comment Matching

Comment search uses broad matching to surface issues where any comment contains the search terms, not just exact phrases. This trades precision for recall — in an agent-heavy workflow, missing a relevant issue is costlier than surfacing a few extra results.

### Filter-Only Changes Skip Refetch

When the user changes inbox filters (e.g., status, assignee) without changing the underlying data query, the inbox avoids refetching from the server. This optimization prevents unnecessary loading states during rapid filter toggling.
