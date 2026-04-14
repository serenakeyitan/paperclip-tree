---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No existing node covers comment-triggered wake mechanics or wake performance optimization — heartbeat-run-orchestration covers scheduled heartbeats but not comment-wake efficiency specifically.
---
# Comment Wake Efficiency

How the backend optimizes agent wake-ups triggered by comments, ensuring low-latency response to human and agent interactions in issue threads.

**Source:** `server/src/services/` (comment and wake-related services)

## Key Decisions

### Comment-Triggered Wake Path

When a comment is posted on an issue that has an assigned agent, the system triggers an immediate wake rather than waiting for the next scheduled heartbeat. This path is optimized for latency — the goal is for agents to respond to comments as quickly as possible, since comment interactions feel like chat.

### Wake Deduplication

Multiple rapid comments on the same issue do not spawn multiple concurrent wakes. The system deduplicates wake triggers within a short window, ensuring only one wake fires per burst of activity. This prevents resource waste and race conditions from rapid-fire comments.

### Performance Optimization

The comment-wake path is a critical latency path (PAP-874). Optimizations target the time between comment creation and agent execution start, minimizing unnecessary work in the wake pipeline.

## Boundaries

- The **heartbeat protocol** and scheduled wakes are covered in `product/agent-model` and `engineering/backend/heartbeat-run-orchestration`. This node covers the **comment-triggered** fast path.
- **Execution workspace** setup after wake is covered in `engineering/execution-workspaces`.
