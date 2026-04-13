---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Claude Local Adapter node documents session resume and system prompt injection separately but does not capture the constraint that --append-system-prompt-file must be skipped on resumed sessions to avoid duplicate prompt injection.
---
Update the existing Claude Local Adapter node's **Session resume** bullet or **Key Decisions** section to add:

### System Prompt Skipped on Resume

`--append-system-prompt-file` is only passed on **new** sessions. When resuming via `--session-id`, the flag is omitted because the system prompt was already injected in the original session. Re-appending it would duplicate instructions and waste context window tokens.

**Rationale:** The system prompt file contains Paperclip's agent instructions and skill definitions. Claude Code persists these across the session, so re-injecting them on resume is redundant and can cause confusing duplicate behavior.
