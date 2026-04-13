---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Express 5 routing syntax (wildcard *name pattern) is a cross-cutting backend convention that affects all route definitions — existing backend node doesn't mention Express version.
---
# Express 5 Routing

The Paperclip server runs Express 5 (not Express 4). This affects routing syntax across the entire backend.

## Key Decisions

### Wildcard Syntax Change

Express 5 changes wildcard/path-parameter syntax: `*` becomes `*name` (a named wildcard). All catch-all handlers (e.g., BetterAuth mount, SPA fallback) must use the Express 5 pattern. Using the old `*` syntax will cause runtime errors.

### Migration Constraint

Any new route or middleware mount must use Express 5 patterns. When copying route patterns from external documentation or examples, verify they use Express 5 syntax — most online resources still show Express 4.
