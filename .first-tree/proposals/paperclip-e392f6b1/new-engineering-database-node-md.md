---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The database node mentions 'backup/restore built in' but doesn't cover the new gzip compression or tiered retention policies added by this PR.
---
Add to the database node: backup dumps are now gzip-compressed and governed by configurable tiered retention policies (see `infrastructure/backups/NODE.md`). Add a soft_link to `infrastructure/backups/NODE.md`.
