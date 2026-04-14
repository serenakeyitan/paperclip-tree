---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Codex adapter node documents session resume capability but doesn't cover stale session detection or the error patterns that trigger session reset, which this PR adds.
---
### Stale Session Detection

When resuming a session, certain Codex CLI errors indicate the session is no longer valid. The adapter recognizes these as stale sessions and falls back to starting a fresh session rather than failing.

Known stale-session error patterns:
- `missing-rollout` — the Codex backend no longer has the rollout configuration for the session

This ensures agents recover gracefully from transient backend state changes without manual intervention.
