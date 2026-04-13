---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3383 adds fast mode support to the Codex Local adapter, but the existing node makes no mention of fast mode as a capability or how it is configured.
---
## Fast Mode

- **Fast mode:** Supported. Codex CLI can be invoked in a fast/turbo mode that trades thoroughness for speed. Paperclip exposes this as a configurable option on the adapter, passing the appropriate flag to the Codex child process.
- Fast mode is useful for tasks where latency matters more than exhaustive reasoning — quick edits, simple file operations, or time-boxed subtasks.
- The adapter handles fast mode toggling at spawn time, so sessions started in fast mode stay in fast mode for their duration.
