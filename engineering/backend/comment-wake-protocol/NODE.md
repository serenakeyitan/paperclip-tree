---
title: "Comment Wake Protocol"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Comment Wake Protocol

Event-driven agent wakes triggered by comments on issues, distinct from scheduled heartbeat wakes.

## Key Decisions

### Event-Driven, Not Polled

Comment-triggered agent wakes are immediate and event-driven, not batched into heartbeat cycles. When a comment is posted on an issue assigned to an agent, the agent is woken immediately rather than waiting for the next scheduled heartbeat.

**Rationale:** Comments often represent urgent context changes — a human asking a question, a blocker being discussed, new requirements. Waiting for the next heartbeat cycle introduces unacceptable latency in interactive workflows.

### Debouncing and Deduplication

Rapid sequential comments are debounced (batched into a single wake). If an agent is already running, duplicate wake requests are suppressed. System-generated comments (status changes, automated notifications) do not trigger wakes.

**Rationale:** Without these guards, a burst of comments would spawn redundant agent runs, wasting compute and potentially causing conflicts. Selective wake filtering ensures agents only wake for meaningful human or agent input.

### Shared Execution Infrastructure

Comment wakes use the same execution infrastructure as heartbeat runs but inject comment-specific context into the agent's wake payload. This avoids duplicating the run lifecycle machinery.
