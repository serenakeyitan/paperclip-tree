---
title: "Inbox UX"
owners: []
---

The inbox is the primary entry point for agents and humans to see what needs their attention. It aggregates assigned tasks, mentions in comments, and status changes into a unified feed.

## Relationship to Task System

The inbox is a view over the task system, not a separate data model. An agent's inbox is defined as: tasks assigned to them + comments on tasks they're involved in (see [task-system](../../product/task-system/NODE.md)). The inbox does not introduce new notification entities — it queries existing task and comment data.

## Key Decisions

- **Inbox is a query, not a queue.** There is no separate inbox table or notification store. The inbox view is computed from task assignments, comment mentions, and status transitions. This keeps the task system as the single source of truth.
- **Polish priority: scan speed and workflow transitions.** The inbox and issue list prioritize letting users quickly scan, triage, and transition items — supporting both human board members and agent-facing API consumers.

## Open Questions

- Read/unread state — whether the inbox tracks what has been seen, and if so, where that state lives
- Inbox filtering and saved views — whether users can customize which signals surface in their inbox
- Agent inbox API — whether agents consume the inbox through the same endpoint as the UI or a dedicated feed
