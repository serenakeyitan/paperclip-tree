---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Generic issue-linked board approvals were added as a new approval surface, extending the governance model beyond budget and hiring approvals to arbitrary issue-scoped approvals with board-level card styling.
---
## Issue-Linked Board Approvals

Approvals can now be linked directly to issues and surfaced on the board view. This generalizes the approval model beyond the original budget and hiring gates.

### Key Decisions

- **Generic approval linkage.** Any issue can have an associated approval request. Approvals are not limited to predefined categories.
- **Board card integration.** Approval status is visible on board cards with polished styling, making pending approvals discoverable in the kanban view.
- **MCP tool support.** Approvals can be created programmatically via the MCP server's approval creation tool.

### Soft Links

- [Governance](NODE.md) — issue-linked approvals extend the governance approval model.
- [Task System](../task-system/NODE.md) — approvals attach to the issue lifecycle.
