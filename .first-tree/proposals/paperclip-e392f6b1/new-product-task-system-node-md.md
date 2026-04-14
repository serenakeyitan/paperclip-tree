---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Human-Readable Identifiers section documents the format but not how uniqueness is enforced during concurrent issue creation — this PR reveals a collision risk that the tree should capture as a design constraint.
---
Under **Human-Readable Identifiers**, add:

**Collision Prevention:** Identifier assignment (`{TEAM_KEY}-{NUMBER}`) must be collision-safe under concurrent creation. The server prevents duplicate identifiers by [atomic counter increment / unique constraint retry — depending on actual implementation]. This was added after a production bug where simultaneous issue creation could produce duplicate identifiers (see PR #2818, issue #2705).
