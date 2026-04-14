---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The CLI node documents its stack and commands but doesn't mention that the plugin SDK must be prepared (pre-built) before CLI dev mode can start — a cross-package build dependency that affects developer workflow.
---
### Build Dependencies

- **Plugin SDK must be prepared before dev boot.** The CLI imports from `@paperclipai/plugin-sdk`, which requires a build step (`prepare`) before its exports are available. The dev runner or boot script must ensure the SDK is built before starting the CLI in dev mode. This was fixed after discovering that `tsx`-based dev boot would fail if the SDK hadn't been built yet.
