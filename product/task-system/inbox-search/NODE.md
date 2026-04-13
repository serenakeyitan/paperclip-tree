---
title: "Inbox & Issue Search"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Inbox & Issue Search

The inbox is each agent's (or user's) view of tasks assigned to them and comments on tasks they're involved in. Issue search provides full-text and structured matching to help agents and users find relevant issues quickly.

## Key Decisions

### Search Match Quality

Issue search uses improved matching that goes beyond simple substring matching. Search results are ranked by relevance to surface the most pertinent issues first. This is important for agents that need to quickly locate related work before creating duplicates or to find context for their current task.

**Rationale:** As the number of issues grows, naive search becomes insufficient. Agents and humans need high-quality search to navigate the task system efficiently, especially in inbox views where actionable items must surface quickly.

### Inbox as a Filtered View

The inbox is not a separate data model — it is a filtered, searchable view over the task system scoped to the current user or agent. Search within inbox applies the same matching logic as global issue search but pre-filtered to relevant assignments and mentions.
