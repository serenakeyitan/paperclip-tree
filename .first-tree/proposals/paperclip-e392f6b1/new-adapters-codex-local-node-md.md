---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The node mentions isCodexUnknownSessionError for session reset decisions but doesn't document the missing-rollout error pattern now recognized as a stale session signal.
---
Add to the **Boundaries** section, expanding the existing bullet about unknown session errors:

- Unknown session errors are detected (`isCodexUnknownSessionError`) and surfaced to the server for session reset decisions. The `missing-rollout` Codex resume error is recognized as a stale session indicator, triggering automatic session reset rather than surfacing as an unhandled failure.
