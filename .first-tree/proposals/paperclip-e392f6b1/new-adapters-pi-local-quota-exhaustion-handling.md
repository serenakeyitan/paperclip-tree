---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Pi adapter node explicitly states 'No quota reporting' but this PR adds quota exhaustion detection and treatment as a failed run — a new error-handling behavior the tree must capture.
---
# Pi Quota Exhaustion Handling

When the Pi runtime returns a quota-exhaustion response, the adapter treats the run as **failed** rather than retrying or hanging. This is a deliberate decision: quota exhaustion is not a transient error, so retrying would waste resources and delay surfacing the problem to the user.

## Key Decisions

### Quota Exhaustion = Failed Run

A quota-exhausted response from Pi is mapped to a terminal failure state. The adapter does not retry, does not fall back to another model, and does not silently swallow the error. The task system sees a failed run and can surface it through normal failure-handling paths (dashboards, notifications).

**Rationale:** Treating quota exhaustion as a recoverable error would create silent loops. Failing fast lets the human or manager agent decide whether to wait, switch providers, or cancel the work.

## Impact on Boundaries

This changes the previous boundary noted in the Pi adapter node ('No quota reporting'). The adapter now detects and reports quota exhaustion, though it still does not manage or configure Pi quotas directly.
