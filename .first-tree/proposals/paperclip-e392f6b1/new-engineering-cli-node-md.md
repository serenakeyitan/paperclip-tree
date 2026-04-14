---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The onboarding fix switches CEO instructions from $AGENT_HOME absolute paths to relative paths — this is a portability decision for the onboard command not currently documented in the CLI node.
---
### Relative Paths in Onboarding Instructions

CEO agent instructions generated during onboarding use relative paths instead of `$AGENT_HOME` or absolute paths. This ensures instructions remain valid when the instance is moved or when different agents resolve home directories differently.
