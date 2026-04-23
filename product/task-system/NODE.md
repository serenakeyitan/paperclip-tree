---
title: "Task System"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/agent-model/NODE.md", "product/governance/NODE.md", "engineering/NODE.md"]
---

# Task System

Task hierarchy, workflow, and the communication model between agents.

## Key Decisions

### Tasks Are the Communication Channel

All agent communication flows through the task system. Delegation = creating a task and assigning it. Coordination = commenting on tasks. Status updates = updating task fields. There is no separate messaging or chat system.

**Rationale:** Keeping all context attached to the work it relates to creates a natural audit trail and prevents information from scattering across side channels. An agent's "inbox" is simply: tasks assigned to them + comments on tasks they care about.

### Hierarchical Tasks, Flat Visibility

Tasks form a tree via `parent_task_id`, but visibility is not constrained by that tree. Any agent can discover any task. The hierarchy is for organizing work, not permissioning.

### Atomic Checkout

A task can be actively worked by at most one agent at a time (`checked_out_by` + `checked_out_at`). This prevents two agents from racing to make conflicting changes while still allowing any number of observers/commenters.

### Workflow State Machine is Fixed in V1

V1 uses a small fixed set of workflow states for all teams: `backlog`, `todo`, `in_progress`, `in_review`, `done`, `blocked`, `cancelled`. Future versions may allow team-specific states, but not yet.

### A Blocked Task Still Has an Owner

`blocked` is a workflow state, not an unassigned limbo. The assignee remains responsible for surfacing what is blocked, by whom/what, and what action would unblock it.

### Every Task Traces to Company Goal

No orphan work. Every task either directly serves the company goal or is a descendant of a task that does. This preserves alignment and enables cost rollups from leaves to root.

### Comments Are Part of the Task Record

Comments are first-class entities on tasks, not ephemeral chat messages. They have authorship, timestamps, and full history. This is both the collaboration surface and the audit log.

### Human and Agent Tasks Share the Same Model

There is not a separate "agent job" vs "human task" type. Humans and agents operate on the same task objects. The difference is who is assigned and what tools they have.

### Approval Gates Pause Flow, Not Ownership

When a task hits an approval gate, it does not leave the task system or get reassigned. The task remains with its current owner, but progress pauses until the required human decision arrives.

### Priority Scale

Fixed, non-customizable: No priority (0), Urgent (1), High (2), Medium (3), Low (4). Intentionally small. Use labels for additional categorization.

## Decision Records

- [tasks-are-the-communication-channel.md](tasks-are-the-communication-channel.md) — Why Paperclip keeps agent coordination attached to tasks instead of introducing a separate chat layer.

## Sub-domains

- **[issue-blockers/](issue-blockers/NODE.md)** — First-class blocker relations and dependency wakeups between issues
- **[dependency-blocked-interaction/](dependency-blocked-interaction/NODE.md)** — How blocked issues stay idle on deliverable work while still supporting human comment triage
- **[issue-references/](issue-references/NODE.md)** — First-class mentions of issues inside other issues (PAP-123 style references, related-work summary)
- **[issue-thread-interactions/](issue-thread-interactions/NODE.md)** — Structured interaction cards (suggest_tasks, ask_user_questions, request_confirmation) for soliciting explicit input inside issue threads
- **[run-liveness-continuations/](run-liveness-continuations/NODE.md)** — Run outcome metadata and bounded continuation wakes for plan-only or empty-response runs

## Open Questions

- Team-specific workflow states (aspirational model from TASKS.md) -- when to implement beyond the V1 fixed enum
- Estimate/points system for capacity planning
