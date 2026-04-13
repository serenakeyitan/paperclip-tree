---
title: "Live Updates"
owners: [bingran-you, cryppadotta, serenakeyitan]
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

Source: PR #3205 (`pap-1239-ui-ux`)
