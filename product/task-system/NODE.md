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

**Rationale:** Keeping all context attached to the work it relates to creates a natural audit trail and prevents information from scattering across side channels. An agent's "inbox" is simply: tasks assigned to them + comments on tasks they're involved in.

### All Work Traces to the Goal

Every piece of work must trace back to the company's top-level initiative through a chain of parent tasks. If you can't explain why a task matters to the company goal, it shouldn't exist. The full hierarchy: **Initiative** (company goal) > Projects > Milestones > Issues > Sub-issues.

**Rationale:** This is what keeps autonomous agents aligned. At any point, any agent can answer "why am I doing this?" by walking up the task tree. This prevents drift and makes cost attribution meaningful.

### Single-Assignee Model

Each task has at most one assignee at a time. This is deliberate: clear ownership prevents diffusion of responsibility. For collaborative work involving multiple agents, use sub-issues with different assignees.

**Rationale:** Single ownership eliminates ambiguity about who is accountable. Combined with atomic checkout, it prevents conflicts at the design level rather than resolving them after the fact.

### Atomic Task Checkout

Claiming a task (`in_progress` transition) is an atomic database operation. If another agent already claimed it, the request fails with a 409 conflict identifying the current owner. No optimistic locking or CRDTs needed.

**Rationale:** The single-assignment model + atomic checkout prevents conflicts at the design level. This is simpler and more reliable than distributed coordination schemes.

### Issue Status Workflow

Fixed status set for V1: `backlog`, `todo`, `in_progress`, `in_review`, `done`, `blocked`, `cancelled`. Terminal states: `done`, `cancelled`.

Side effects are automatic: entering `in_progress` sets `started_at`, entering `done` sets `completed_at`, entering `cancelled` sets `cancelled_at`.

**Aspirational model (TASKS.md):** Team-specific workflow states within fixed categories (Triage, Backlog, Unstarted, Started, Completed, Cancelled). Custom states like "In QA" can be added within categories. This is the target model; V1 uses the simpler fixed enum.

### Human-Readable Identifiers

Issues use `{TEAM_KEY}-{NUMBER}` format (e.g., `ENG-123`) instead of UUIDs for human communication. If an issue moves between teams, it gets a new identifier and the old one is preserved.

### Work Artifacts

Paperclip manages task-linked work artifacts: **issue documents** (rich-text plans, specs, notes attached to issues with a stable workflow key like `plan`, `design`, `notes`) and **file attachments**. Full delivery infrastructure (repos, deployments) remains the agent's domain.

**Rationale:** Agents need to produce visible outputs beyond status updates. Documents and attachments make work tangible without pulling Paperclip into build pipeline territory.

### No Automatic Recovery

When an agent crashes mid-task, Paperclip does **not** auto-reassign or auto-release the task. Stale tasks (in `in_progress` with no recent activity) are surfaced through dashboards. Recovery is manual/explicit.

**Rationale:** Automatic recovery hides failures. Good visibility lets the right entity decide what to do. A project manager agent whose job is to monitor for stale work is the emergent pattern, not a built-in behavior.

### Priority Scale

Fixed, non-customizable: No priority (0), Urgent (1), High (2), Medium (3), Low (4). Intentionally small. Use labels for additional categorization.

## Decision Records

- [tasks-are-the-communication-channel.md](tasks-are-the-communication-channel.md) — Why Paperclip keeps agent coordination attached to tasks instead of introducing a separate chat layer.

## Sub-domains

- **[issue-blockers/](issue-blockers/NODE.md)** — First-class blocker relations and dependency wakeups between issues
- [agent-inbox-lite/](agent-inbox-lite/) — Lightweight per-agent inbox surface including routine-execution issues

## Open Questions

- Team-specific workflow states (aspirational model from TASKS.md) -- when to implement beyond the V1 fixed enum
- Estimate/points system for capacity planning
