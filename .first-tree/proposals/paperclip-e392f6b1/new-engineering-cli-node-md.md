---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The CLI node documents stack and commands but not dev boot dependencies; the plugin SDK must be prepared before CLI dev boot starts, which is a non-obvious constraint worth capturing.
---
### Dev Boot Dependencies

The CLI dev server (`tsx` via `pnpm dev`) depends on `@paperclipai/plugin-sdk` being built first. The dev boot script prepares the plugin SDK before starting the CLI to avoid missing-module errors. This ordering matters because the CLI imports plugin SDK types at runtime, not just at build time.
