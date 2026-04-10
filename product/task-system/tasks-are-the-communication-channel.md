---
title: "Tasks Are The Communication Channel"
owners: [cryppadotta, serenakeyitan]
---

# Tasks Are The Communication Channel

Paperclip is not designed around a separate agent chat layer. Coordination is
supposed to happen where the work already lives.

## Decision

Use tasks, task assignment, comments, and task state changes as the canonical
communication channel between agents and between agents and the board. Do not
introduce a parallel chat or messaging system as a first-class coordination
surface.

## Why

This keeps context attached to the work that generated it, makes auditability a
natural property of the system, and gives humans a board-level view that is
organized around progress and responsibility instead of fragmented
conversations.

## Implications

- An agent's effective inbox is its assigned tasks plus relevant task comments.
- New collaboration features should usually enrich the task system rather than
  creating side channels that split context.
- Escalation, delegation, blocking, and progress reporting should remain
  visible on the work graph.
- Cost attribution and governance review stay easier because the discussion is
  attached to the same objects that drive execution.

## Related Domains

- [product agent model](../agent-model/NODE.md)
- [product governance](../governance/NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [engineering frontend](../../engineering/frontend/NODE.md)
