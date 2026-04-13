---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR introduces first-class issue blockers (blocking/blocked-by relationships) with automatic dependency wakeups — this was only an open question in task-system and is now a concrete, shipped feature with significant architectural implications for agent coordination.
---
# Issue Links & Dependency Wakeups

Issues can declare directed relationships to other issues — primarily **blocking** and **blocked-by**. When a blocking issue completes, dependent issues are automatically woken up via agent wakeup events, enabling autonomous agents to resume work without manual intervention.

## Key Decisions

### Blocking as a First-Class Relationship

Blocking/blocked-by is the primary link type. It is semantically meaningful: a blocked issue cannot proceed until all its blockers are resolved. This is enforced through the UI (blocker fields visible in the issue sidebar) and through agent coordination (wakeup events on blocker completion).

**Rationale:** Agents working autonomously need a reliable signal that their dependencies are met. Without first-class blocking semantics, agents would need to poll or rely on human coordination to know when to resume.

### Automatic Dependency Wakeups

When a blocking issue transitions to a terminal state (`done` or `cancelled`), the system emits wakeup events to all issues that were blocked by it. This triggers assigned agents to re-evaluate and potentially resume work.

**Rationale:** Autonomous agent coordination requires event-driven signaling. Polling for dependency resolution wastes cycles and introduces latency. Wakeup events make the dependency graph an active coordination mechanism.

### Sub-Issues Default to Parent Workspace

Sub-issues automatically inherit their parent issue's workspace assignment, reducing friction when decomposing work.

## Boundaries

This node covers the link semantics, blocking model, and wakeup mechanism. The task system hierarchy and status workflow are documented in the parent [task-system](../NODE.md) node. Frontend rendering of blockers and inline sub-issues is a UI concern.

## Open Questions

- Additional link types beyond blocking (related, duplicate) and their semantics
- Whether circular blocking dependencies are detected and prevented at creation time
- Wakeup behavior when a blocker is cancelled vs. completed
