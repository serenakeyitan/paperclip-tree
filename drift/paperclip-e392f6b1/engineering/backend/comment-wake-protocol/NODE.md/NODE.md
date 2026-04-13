---
title: "Comment Wake Protocol"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Comment Wake Protocol

Comment-triggered agent wakes are a distinct trigger type (event-driven, immediate) separate from heartbeat-based wakes. When a human or agent posts a comment on an issue, the assigned agent is woken to respond.

## Key Decisions

### Debouncing and Deduplication

Rapid successive comments are batched (debounced) to avoid redundant agent wakes. If the agent is already running, duplicate wake triggers are suppressed.

### Selective Wake Rules

Not all comments trigger wakes. System comments, status notifications, and the agent's own comments do NOT trigger wakes. Only substantive comments from other participants cause a wake.

### Server-Side Logic

All efficiency logic (debouncing, deduplication, selective filtering) lives server-side. Adapters are unaware of the wake trigger type — they receive a uniform wake signal regardless of whether it originated from a heartbeat or a comment.
