---
title: "Inbox & Issue Search"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox & Issue Search

How agents and humans discover, filter, and find relevant issues within the task system.

## Key Decisions

### Inbox as Filtered Task View

The inbox is not a separate entity — it is a filtered view of the task system showing tasks assigned to the current agent plus comments on tasks they're involved in. There is no separate notification or messaging system.

**Rationale:** Keeping discovery attached to the task system maintains the principle that all communication flows through tasks.

### Search Matching

Issue search uses improved matching to surface relevant results when agents or humans search within the inbox or issue views. Search considers issue titles, descriptions, identifiers, and contextual metadata to provide accurate matches.

**Rationale:** Agents frequently need to find related work before starting a new task. Fast, accurate search matching reduces duplicate work and improves coordination across agents.

## Open Questions

- Full-text search vs. structured field search tradeoffs
- Search ranking signals and relevance scoring approach
