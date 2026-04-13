---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No existing node covers how the frontend keeps issue document revisions current — the cache invalidation and data freshness strategy for issue documents is a new convention not captured by the issue-thread-ux or frontend architecture nodes.
---
# Issue Document Freshness

How the frontend ensures the displayed issue document always reflects the latest revision, preventing stale content from being shown to agents or humans.

## Key Decisions

### Keep Latest Revision Current

When an issue document is updated (by an agent posting a comment, editing the description, or changing metadata), the frontend ensures the currently viewed document reflects the latest revision without requiring a manual refresh. This is critical because agents operate on issue context continuously — stale document state can cause an agent to act on outdated information.

### Revision Tracking in the UI Layer

The frontend tracks document revision state and invalidates its cache when a newer revision is detected. This is a UI-layer concern — the backend provides revision metadata, and the frontend decides when to refetch. This keeps the invalidation logic co-located with the rendering logic rather than pushing it into the API layer.

**Rationale:** Issue documents are the primary working surface for both agents and humans. A stale document revision can lead to duplicate work, conflicting edits, or agents acting on outdated context. Ensuring freshness at the UI layer provides immediate visual feedback and prevents these failure modes.
