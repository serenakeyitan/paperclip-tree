---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The infrastructure domain node needs to list the new backups/ subdomain alongside deployment/, ci-cd/, and testing/.
---
Add `backups/` to the list of infrastructure subdomains, e.g.: `- **[backups/](backups/NODE.md)** — Gzip-compressed database backups with tiered retention and admin UI controls.`
