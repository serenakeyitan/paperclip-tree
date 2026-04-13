---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Execution policies (configurable sign-off-required gates per action category) are a new governance feature not covered by the existing governance node.
---
# Execution Policies

Configurable sign-off-required policies that designate action categories requiring human approval before execution. The Board defines which actions need sign-off; enforcement happens at the server layer using the same blocking model as existing approval gates.

## Key Decisions

### Server-Side Enforcement

Execution policies are enforced at the server layer, not the adapter or UI layer. This ensures policies cannot be bypassed regardless of how an action is initiated (UI, API, MCP, or CLI).

### Board-Designated Categories

The Board (governance authority) designates which action categories require approval. This keeps governance decisions centralized with the appropriate authority rather than distributed across individual agent configurations.
