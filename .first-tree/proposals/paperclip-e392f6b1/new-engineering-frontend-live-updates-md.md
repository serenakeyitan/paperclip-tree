---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: PR introduces live update infrastructure for issue workflow surfaces; the frontend node documents React Query for server state but has no node covering real-time/live update patterns (SSE, WebSocket, polling, or optimistic update strategies).
---
## Live Updates

Real-time update pattern used across the Paperclip frontend to keep issue workflow surfaces current without manual refresh.

### Key Decisions

#### Live Update Mechanism

The frontend uses a live update strategy to reflect issue state changes (status transitions, new comments, assignment changes) in near-real-time. This was introduced as part of the issue workflow refinement (PR #3222) to ensure that board views, detail panels, and inbox surfaces stay synchronized with backend state.

**Rationale:** Autonomous agents move fast — an issue can transition through multiple states in seconds. Stale UI creates confusion for human board operators who need accurate visibility to exercise governance. Live updates close the gap between agent activity and human awareness.

### Integration with React Query

Live updates integrate with the existing React Query cache invalidation model rather than introducing a separate state synchronization layer. This keeps the "no global state library" principle intact while adding reactivity.

### Open Questions

- Specific transport mechanism (SSE vs WebSocket vs polling) and its configuration
- Backpressure and throttling strategy for high-activity companies with many concurrent agents
