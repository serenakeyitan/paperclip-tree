---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR includes three CI fixes (2c8cb7f, ac47382, 1e4d252) handling manifest changes without lockfile and MCP PR lockfile management — these are CI pipeline decisions worth capturing if the node doesn't already cover them.
---
### Manifest-Only PRs Without Lockfile

CI supports PRs that change only the package manifest (e.g., MCP tool definitions) without requiring lockfile changes. The pipeline restores the lockfile to the PR base when lockfile changes are not relevant to the changeset.
