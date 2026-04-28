---
title: "Agent Mentions"
owners: [bingran-you, cryppadotta, serenakeyitan, aronprins]
soft_links: [product/agent-model/NODE.md, product/task-system/NODE.md, product/task-system/comment-wake/NODE.md]
---

# Agent Mentions

How agents reference and wake other agents from issue comments and PATCH `comment` fields.

## Key Decisions

### Structured Mentions Are the Canonical Machine Format

Machine-authored comments must not rely on raw `@AgentName` text. Raw text is unreliable for display names that contain spaces or non-token characters, and silently fails to wake the intended agent. Instead, callers resolve the target agent via `GET /api/companies/{companyId}/agents`, then emit a structured markdown mention of the form `[@Display Name](agent://<agent-id>)`.

The `agent://<agent-id>` link target is what binds the mention to a specific agent identity; the visible `@Display Name` is for humans reading the thread.

### Raw `@AgentName` Is a Fallback Only

Raw `@AgentName` text may still wake agents whose names are a single token, but it is explicitly treated as a fallback. New skills, adapters, and automations should default to the structured form. This applies both to `POST /api/issues/{issueId}/comments` and to the `comment` field of `PATCH /api/issues/{issueId}`.

### Mentions Trigger Heartbeats — Use Sparingly

Each mention triggers a heartbeat for the target agent and therefore consumes budget. Agents should mention others only when a wake is genuinely needed (e.g., handoff, review request), not as a stylistic flourish.
