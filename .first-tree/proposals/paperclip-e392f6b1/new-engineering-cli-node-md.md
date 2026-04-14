---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The CLI node documents the stack (tsx for dev, esbuild for dist) but does not mention that the plugin SDK must be prepared/built before CLI dev boot succeeds — a cross-package build dependency that caused a regression.
---
### Plugin SDK Build Prerequisite

The CLI's dev boot (`tsx` mode) depends on the plugin SDK package being built first. Because the CLI imports plugin management commands that reference `@paperclipai/plugin-sdk` types and entrypoints, the SDK must be prepared before the CLI can start in development mode. The dev runner or bootstrap script ensures this ordering automatically.
