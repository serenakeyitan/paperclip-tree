---
title: "Issue Links & Dependency Wakeups"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Links & Dependency Wakeups

Issues can declare directed relationships to other issues — primarily **blocking** and **blocked-by**. When a blocking issue completes, dependent issues are automatically woken up via agent wakeup events, enabling autonomous agents to resume work without manual intervention.

## Key Decisions

### Direct Link Navigation

Issue-to-issue links are optimized for fast traversal. When an agent or user views an issue, linked issues are immediately navigable without additional lookups.

### Blocking as a First-Class Relationship

Blocking/blocked-by is the primary link type. It is semantically meaningful: a blocked issue cannot proceed until all its blockers are resolved. This is enforced through the UI (blocker fields visible in the issue sidebar) and through agent coordination (wakeup events on blocker completion).

**Rationale:** Agents working autonomously need a reliable signal that their dependencies are met. Without first-class blocking semantics, agents would need to poll or rely on human coordination to know when to resume.

### Automatic Dependency Wakeups

When a blocking issue transitions to a terminal state (`done` or `cancelled`), the system emits wakeup events to all issues that were blocked by it. This triggers assigned agents to re-evaluate and potentially resume work.

**Rationale:** Autonomous agent coordination requires event-driven signaling. Polling for dependency resolution wastes cycles and introduces latency. Wakeup events make the dependency graph an active coordination mechanism.

### Sub-Issues Default to Parent Workspace

Sub-issues automatically inherit their parent issue's workspace assignment, reducing friction when decomposing work.

## Open Questions

- Additional link types beyond blocking (related, duplicate) and their semantics
- Whether circular blocking dependencies are detected and prevented at creation time
- Wakeup behavior when a blocker is cancelled vs. completed
