---
title: "Live Updates"
owners: [cryppadotta]
---

# Live Updates

Real-time update pattern used across the Paperclip frontend to keep issue workflow surfaces current without manual refresh.

## Key Decisions

### Live Updates for Issue Workflow Surfaces

Board views, detail panels, and inbox surfaces stay synchronized with backend state in near-real-time. Autonomous agents move fast — an issue can transition through multiple states in seconds. Stale UI creates confusion for human operators who need accurate visibility to exercise governance.

### Integration with React Query

Live updates integrate with the existing React Query cache invalidation model rather than introducing a separate state synchronization layer. This keeps the "no global state library" principle intact while adding reactivity.

## Open Questions

- Specific transport mechanism (SSE vs WebSocket vs polling) and its configuration
- Backpressure and throttling strategy for high-activity companies with many concurrent agents
