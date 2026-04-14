---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR dims paused agents in both the list and org chart views — this establishes a visual convention for agent pause state that the agent model node (which documents agent statuses and org structure) does not currently capture.
---
### Paused Agent Visual Treatment

Paused agents are visually dimmed in both the agent list and org chart views. This makes the operational state of the org immediately scannable — agents that are paused (by budget hard-stop or board action) are distinguishable at a glance without opening each agent's detail view.
