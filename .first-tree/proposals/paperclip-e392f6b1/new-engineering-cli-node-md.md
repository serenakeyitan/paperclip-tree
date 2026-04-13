---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR fixes a dev boot ordering issue where the plugin SDK must be built before the CLI can start in dev mode — a build dependency not captured in the CLI node.
---
Update the Key Decisions section to add:

### Plugin SDK Build Before Dev Boot

The CLI's dev mode (`tsx`-based) requires the plugin SDK package (`@paperclipai/plugin-sdk`) to be built before the CLI can start. The dev boot script explicitly prepares the plugin SDK as a prerequisite. This was added after discovering that running the CLI in dev mode without a pre-built SDK caused import resolution failures, since the SDK's compiled output is referenced at runtime.
