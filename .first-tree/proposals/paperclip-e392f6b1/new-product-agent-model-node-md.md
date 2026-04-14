---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR dims paused agents in list and org chart views — the agent model node lists statuses but doesn't capture how paused state is visually communicated in the UI, which is a product-level decision about agent status visibility.
---
### Visual Status Differentiation

Paused agents are visually dimmed in both the agent list and org chart views. This makes agent operational status immediately scannable without requiring users or agents to inspect individual agent details. The visual treatment reinforces the governance model — paused agents are clearly distinguished from active ones.
