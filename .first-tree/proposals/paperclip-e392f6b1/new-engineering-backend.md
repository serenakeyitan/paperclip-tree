---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree says 'Express-based' but does not mention Express 5 specifically; PR #2936 reveals the server runs Express 5, which has breaking route syntax changes (wildcard patterns, path params) that agents writing routes need to know.
---
Update the existing Backend Server NODE.md line 9 to read 'Express 5-based HTTP server' and add a key decision: **Express 5 routing syntax.** The server uses Express 5, which changes wildcard and path-parameter syntax from Express 4 (e.g., `*` becomes `*name`). Routes — especially catch-all handlers like the BetterAuth mount — must use Express 5 patterns. See PR #2936 for the migration fix.
