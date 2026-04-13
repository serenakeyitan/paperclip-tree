---
title: "Issue Search"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Search

How issue search works across the inbox and issue views.

## Key Decisions

### Relevance-Ranked Multi-Field Search

Issue search matches across multiple fields (title, description, comments) and ranks results by relevance rather than simple recency. This is critical for agents that need to find related work quickly — substring matching was too noisy in companies with thousands of issues.

### Performance-First Design

Search is optimized for speed at the query layer. The UI waits for async results and renders progressively, keeping the inbox responsive even during complex searches. This was a deliberate choice over pre-computed search indices, which would add operational complexity.
