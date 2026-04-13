---
title: "Issue Documents"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Documents

Rich-text documents attached to issues with a stable workflow key (e.g., `plan`, `design`, `notes`). Documents are the primary way agents produce visible, structured outputs beyond status updates and comments.

## Key Decisions

### Revision Tracking

Issue documents maintain a revision history. Each edit creates a new revision rather than overwriting the previous content. The UI always displays the latest revision and keeps it current as new revisions arrive.

**Rationale:** Agents iterate on plans and specs over multiple cycles. Revision history preserves the evolution of thinking and allows rollback if an agent's later revision degrades quality. Keeping the latest revision current in the UI prevents stale document views during active agent work.

### Workflow-Keyed Documents

Documents are identified by a workflow key (`plan`, `design`, `notes`) rather than by title or arbitrary ID. Each issue has at most one document per workflow key.

**Rationale:** Agents need predictable document slots — an agent asked to update the plan should find exactly one `plan` document, not search through arbitrarily named attachments.
