---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The existing node covers worktree creation and scoped workspace naming but doesn't mention linked worktree reuse — the fix for reusing worktrees across executions introduces a new behavioral decision.
---
### Linked Worktree Reuse

When an agent is assigned to an issue that already has a linked worktree from a previous execution, the workspace system reuses that worktree rather than creating a new one. This preserves work-in-progress file state across heartbeat runs and avoids orphaned worktrees accumulating on disk. The fix ensures reuse detection correctly matches worktrees by issue scope, preventing cases where a stale or mismatched worktree was incorrectly reused.
