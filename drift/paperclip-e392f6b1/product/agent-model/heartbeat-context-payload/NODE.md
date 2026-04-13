---
title: "Heartbeat Context Payload"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Heartbeat Context Payload

Defines what data Paperclip bundles into the heartbeat-context response that agents receive during invocation. The agent-model node establishes the thin-ping vs. fat-payload spectrum; this node specifies what the fat payload actually contains.

## Key Decisions

### Attachment Metadata in Context

The heartbeat-context response includes metadata for file attachments associated with the agent's assigned issues. This means agents can reason about attached files (names, types, sizes, URLs) without making separate API calls to discover them.

**Rationale:** Agents frequently need to reference files attached to their tasks — design assets, logs, specifications. Including attachment metadata in the heartbeat payload eliminates a round-trip and ensures agents are aware of all relevant context from the start of their execution cycle.

### Payload Composition

The heartbeat-context response is an evolving bundle. As of this decision, it includes:
- Issue details (title, description, status, priority)
- Comments and thread context
- Attachment metadata (file name, type, size, URL)
- Project and company context

New context fields are added as agent needs are discovered. The design favors inclusion over minimalism — it is better for agents to have context they don't need than to lack context they do.

## Cross-References

- The **heartbeat protocol** (when/how to fire, thin vs. fat modes) is defined in `product/agent-model`.
- The **heartbeat run orchestration** (execution lifecycle) is covered in `engineering/backend/heartbeat-run-orchestration`.
- **Attachment storage** is managed by the backend storage provider system (`engineering/backend`).
