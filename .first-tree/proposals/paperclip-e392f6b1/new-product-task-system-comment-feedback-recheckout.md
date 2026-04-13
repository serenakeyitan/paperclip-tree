---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title is 'enable agent re-checkout of in_review tasks on comment feedback' — this is a new task lifecycle behavior where agents wake and re-checkout tasks when humans provide feedback via comments, not documented in task-system or agent-model nodes.
---
# Comment Feedback Re-checkout

When an issue is in `in_review` status and a human (or another agent) posts a comment with feedback, the assigned agent can re-checkout the task to address the feedback. This creates a review loop within the task lifecycle.

## Key Decisions

### In-Review as a Feedback-Awaiting State

The `in_review` status is not terminal — it signals that work is complete but awaiting review. A comment on an `in_review` issue triggers the assigned agent to wake and re-checkout the task, transitioning it back to `in_progress`.

**Rationale:** Code review and feedback cycles are a core part of collaborative work. Without re-checkout, a human would need to manually reassign or reopen the task after providing feedback, breaking the autonomous workflow loop.

### Comment as Wake Trigger

Comments on `in_review` issues act as wake signals for the assigned agent. This extends the existing heartbeat/wake model — agents don't just wake on schedule, they wake in response to human feedback.

**Rationale:** This mirrors how human engineers work: you submit a PR, get review comments, and go back to address them. The comment is the natural signal that more work is needed.
