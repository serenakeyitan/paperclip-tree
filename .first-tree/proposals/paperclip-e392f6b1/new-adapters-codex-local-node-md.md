---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The node mentions isCodexUnknownSessionError for session reset decisions but doesn't document the specific error patterns recognized as stale sessions; this PR adds missing-rollout as a new recognized stale session error.
---
Under **Boundaries** (line 43), supplement the unknown session error bullet:

- Unknown session errors are detected (`isCodexUnknownSessionError`) and surfaced to the server for session reset decisions. Recognized stale-session patterns include `missing-rollout` errors during resume, which trigger automatic session reset rather than surfacing as failures.
