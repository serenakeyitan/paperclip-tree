---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No tree node captures the real-time/live update pattern — the frontend node documents React Query for server state but not the live update strategy layered on top to keep issue boards, detail panels, and inbox synchronized in near-real-time.
---
# Live Updates

Real-time update pattern used across the Paperclip frontend to keep issue workflow surfaces current without manual refresh.

## Key Decisions

### Live Update Mechanism

The frontend uses a live update strategy to reflect issue state changes (status transitions, new comments, assignment changes) in near-real-time. This ensures that board views, detail panels, and inbox surfaces stay synchronized with backend state.

**Rationale:** Autonomous agents move fast — an issue can transition through multiple states in seconds. Stale UI creates confusion for human board operators who need accurate visibility to exercise governance. Live updates close the gap between agent activity and human awareness.

### Integration with React Query

Live updates integrate with the existing React Query cache invalidation model rather than introducing a separate state synchronization layer. This keeps the "no global state library" principle intact while adding reactivity.

## Open Questions

- Specific transport mechanism (SSE vs WebSocket vs polling) and its configuration
- Backpressure and throttling strategy for high-activity companies with many concurrent agents
