---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The existing node describes harness-level auto-checkout as a decision but doesn't capture that checkout is now fully server-executed before the adapter session starts, eliminating agent-side checkout failures.
---
### Server-Side Checkout Execution

Auto-checkout on scoped wake is now fully server-executed. The server performs the checkout before the agent session starts, ensuring the worktree is ready with the correct issue context before the adapter's execute method is invoked. This eliminates a class of boot failures where agents could fail or skip the checkout call, and reinforces the harness-level automation decision — the agent never needs to be aware of checkout mechanics.
