---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Security node covers secret handling but not dependency vulnerability management; this PR establishes a pattern of proactively bumping dependencies to resolve HIGH CVEs (multer 2.1.1) that should be captured.
---
## Dependency vulnerability management

HIGH and CRITICAL CVEs in direct dependencies must be patched promptly by bumping to a fixed version. The project tracks vulnerability alerts and treats unpatched HIGH/CRITICAL CVEs as blockers.

**Rationale:** Multer ≤2.0.x carried multiple HIGH-severity CVEs. Bumping to 2.1.1 resolved them. Leaving known HIGH CVEs unpatched in a production system that handles file uploads (multer's domain) creates unnecessary attack surface.

**Convention:** When a dependency has a published fix for a HIGH or CRITICAL CVE, bump it in a standalone PR with the `fix(security):` commit prefix so the change is auditable and isolated from feature work.
