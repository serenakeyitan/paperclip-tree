---
title: "Attachment Metadata in Heartbeat Context"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Attachment Metadata in Heartbeat Context

The heartbeat-context API response now includes attachment metadata for issues in the agent's current work scope. This gives adapters awareness of files, images, and documents attached to the issues they are working on.

## Key Decisions

### Metadata-Only, Not Content

The heartbeat context includes attachment metadata (filename, MIME type, size, upload timestamp) but not the attachment content itself. Agents that need the actual file data make a separate fetch. This keeps the heartbeat response lightweight — attachment content can be large, and most heartbeat cycles don't need it.

### Scoped to Active Issues

Attachment metadata is included only for issues currently assigned to the requesting agent, not for all issues in the project. This matches the existing heartbeat context scoping model and avoids leaking attachment information across agent boundaries.
