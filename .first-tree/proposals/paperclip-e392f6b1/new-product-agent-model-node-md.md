---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR dims paused agents in list and org chart views — the agent model node documents agent statuses (active/paused/idle/running/error/terminated) but does not capture how paused status is visually communicated in the UI.
---
### Visual Status Indicators

Agent status is visually communicated in list and org chart views. Paused agents are dimmed to provide immediate visual distinction from active agents, reducing cognitive load when scanning agent rosters.
