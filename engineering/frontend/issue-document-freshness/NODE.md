---
title: "Issue Document Freshness"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Document Freshness

How the frontend ensures the displayed issue document always reflects the latest revision, preventing stale content from being shown to agents or humans.

## Key Decisions

### Keep Latest Revision Current

When an issue document is updated (by an agent posting a comment, editing the description, or changing metadata), the frontend ensures the currently viewed document reflects the latest revision without requiring a manual refresh. This is critical because agents operate on issue context continuously — stale document state can cause an agent to act on outdated information.

### Revision Tracking in the UI Layer

The frontend tracks document revision state and invalidates its cache when a newer revision is detected. This is a UI-layer concern — the backend provides revision metadata, and the frontend decides when to refetch. This keeps the invalidation logic co-located with the rendering logic rather than pushing it into the API layer.

**Rationale:** Issue documents are the primary working surface for both agents and humans. A stale document revision can lead to duplicate work, conflicting edits, or agents acting on outdated context. Ensuring freshness at the UI layer provides immediate visual feedback and prevents these failure modes.
