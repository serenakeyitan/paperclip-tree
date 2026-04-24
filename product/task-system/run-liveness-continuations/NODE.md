---
title: "Run Liveness Continuations"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/agent-model/NODE.md", "engineering/backend/heartbeat-run-orchestration/NODE.md", "adapters/claude-local/NODE.md"]
---

# Run Liveness Continuations

Paperclip records **run liveness** as metadata on heartbeat runs. It describes the latest run outcome (for example `completed`, `advanced`, `plan_only`, `empty_response`, `blocked`, `failed`, `needs_followup`) and is **separate from issue status**, which remains authoritative for workflow (`backlog`, `todo`, `in_progress`, `in_review`, `done`, `blocked`, `cancelled`).

## Key Decisions

### Liveness Is Metadata, Not Status

Liveness never replaces the issue status state machine. It is a per-run signal used to decide whether the same agent should be re-woken on the same issue. Continuations do not themselves mark an issue `blocked` or `done`.

### Only Plan-Only and Empty Responses Continue

Of the liveness states, only `plan_only` and `empty_response` can enqueue bounded liveness continuation wakes. Other states (`completed`, `advanced`, `blocked`, `failed`, `needs_followup`) do not auto-continue. Continuations re-wake the same assigned agent on the same issue when the issue is still active and budget and execution policy allow it.

### Continuation Attempt Counter

`continuationAttempt` counts **semantic** liveness continuations for a source run chain. It is deliberately separate from process recovery, queued wake delivery, adapter session resume, and other operational retries, so semantic and operational retries can be bounded independently.

### Continuation Wake Prompts Carry Context

Liveness continuation wake prompts include the attempt number, source run, liveness state, liveness reason, and the instruction for the next heartbeat so the re-woken agent understands why it was brought back and what is expected next.

### Exhaustion Leaves an Audit Comment

If automatic continuations are exhausted, Paperclip leaves an audit comment on the issue so a human or manager can clarify, block, or assign follow-up work. It does not silently give up and does not auto-transition the issue.

### Workspace Provisioning Is Not Progress

Workspace provisioning alone is not treated as concrete task progress when evaluating liveness. Durable progress must appear as tool or action events, issue comments, document or work-product revisions, activity log entries, commits, or tests.

## Execution Contract for Agents

This design is paired with a shared **agent execution contract** exposed via `DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE` in `@paperclipai/adapter-utils/server-utils` and used by every local adapter's default prompt: start actionable work in the same heartbeat, avoid planning-only exits unless planning was requested, leave durable progress with a next action, use child issues for long or parallel delegated work instead of polling agents, sessions, or processes, mark blocked work with unblock owner and action, and respect budget, pause or cancel, approval gates, and company boundaries.

## Boundaries

- **Issue workflow status** remains owned by [Task System](../NODE.md); liveness augments run history but does not replace task state.
- **Heartbeat run execution lifecycle** remains owned by [Heartbeat Run Orchestration](../../../engineering/backend/heartbeat-run-orchestration/NODE.md).
- **Adapter-specific stop reasons and continuation detection** remain owned by adapter nodes such as [Claude Local Adapter](../../../adapters/claude-local/NODE.md).
