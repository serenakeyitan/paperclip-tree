---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The CLI node documents its stack and commands but not its dev-time build dependency on the plugin SDK, which must be prepared before CLI dev boot starts — a cross-package ordering constraint not captured anywhere in the tree.
---
# CLI Dev Boot Ordering

The CLI dev server (`tsx`-based) depends on the plugin SDK being built before it can start. This is a cross-package build ordering constraint that must be maintained in the dev workflow.

## Key Decisions

### Prepare Plugin SDK Before CLI Dev Boot

The CLI imports types and utilities from `@paperclipai/plugin-sdk` at dev time. If the SDK has not been built (or its build output is stale), the CLI dev server will fail to resolve these imports. The fix ensures the SDK is prepared as part of the CLI's dev boot sequence, not as a separate manual step.

This ordering constraint exists because the monorepo uses pnpm workspaces with TypeScript project references. In production builds, the full dependency graph is resolved by the build pipeline. In dev mode, each package runs its own dev server, and cross-package dependencies must be explicitly bootstrapped.

### Convention: Dev Boot Must Declare Cross-Package Dependencies

When a package's dev server depends on another package's build output, that dependency must be declared in the dev boot script (not just in `package.json` dependencies). This prevents silent failures where dev mode works only if packages happen to have been built in the right order from a prior full build.
