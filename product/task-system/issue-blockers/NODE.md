---
title: "Issue Blockers & Dependency Wakeups"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Blockers & Dependency Wakeups

First-class blocking and blocked-by relationships between issues, with automatic agent wakeups when dependencies are resolved.

## Key Decisions

### Directed Blocking Relationships

Issues can declare directed blocking/blocked-by dependencies on other issues. A blocked issue cannot proceed until all its blockers reach a terminal state (`done` or `cancelled`). These are semantic relationships — not just navigation links — that the system actively enforces and reacts to.

**Rationale:** Agents need a reliable, machine-readable way to express dependencies between tasks. Passive link metadata ("related-to") doesn't provide enough signal for autonomous coordination. Blocking semantics give agents clear, unambiguous signals about when they can proceed.

### Automatic Dependency Wakeups

When a blocking issue transitions to a terminal state, the system automatically emits wakeup events to all issues blocked by it. Assigned agents wake and re-evaluate their work without polling.

**Rationale:** Event-driven wakeups replace polling-based dependency tracking. This makes the dependency graph an active coordination mechanism — agents don't need to periodically check whether their blockers are resolved. The system tells them.

### Blocker Display in Issue Sidebar

Blocking and blocked-by relationships are displayed in the issue detail sidebar, giving agents and humans immediate visibility into the dependency graph around any issue.

## Relationship to Issue Links

Blocker relationships are a specific, semantically-loaded type of issue link. General issue links (related-to, duplicate-of) remain as lightweight navigation aids. Blockers are distinguished by their active side effects (status enforcement, wakeup events).
