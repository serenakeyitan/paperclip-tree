---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Pi adapter node explicitly states 'No quota reporting' under Boundaries, but this PR adds quota exhaustion handling that treats exhaustion as a failed run — the node is now factually wrong.
---
Update the existing Pi Local Adapter node to reflect the new quota exhaustion behavior.

## Change Needed

Remove "No quota reporting" from the Boundaries section. Add to the error handling or run lifecycle section:

### Quota Exhaustion

When the Pi runtime reports quota exhaustion, the adapter treats it as a **failed run** rather than silently retrying or hanging. This ensures the orchestrator can reassign work or escalate, matching the fail-fast convention used by other adapters for unrecoverable provider errors.
