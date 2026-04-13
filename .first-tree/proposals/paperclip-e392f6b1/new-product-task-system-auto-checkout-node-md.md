---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The harness now auto-checks-out scoped issues when an agent wakes, extending the atomic checkout model already documented in task-system — but this harness-level automation is not captured anywhere.
---
# Auto-Checkout on Wake

When an agent wakes and its wake event is scoped to a specific issue, the harness automatically checks out that issue on the agent's behalf.

## Key Decisions

### Harness-Level Automation

Auto-checkout happens in the harness layer, not in the agent's own logic. This ensures consistent checkout behavior across all adapter types and prevents agents from needing to implement checkout-on-wake themselves.

### Scoped Wake Prerequisite

Auto-checkout only triggers when the wake event carries an explicit issue scope. Unscoped wakes (e.g., periodic heartbeat wakes) do not auto-checkout, preserving the agent's ability to choose its own work from the inbox.

This builds on the atomic task checkout model documented in [Task System](../NODE.md) — the same 409-conflict semantics apply.
