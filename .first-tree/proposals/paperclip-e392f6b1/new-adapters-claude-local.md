---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The Claude Local Adapter node documents session resume via --session-id but does not capture the constraint that --append-system-prompt-file must be skipped on resumed sessions to avoid re-injecting the system prompt.
---
Update the existing Claude Local Adapter NODE.md — under **Session resume** in Capabilities, add:

> On resumed sessions, `--append-system-prompt-file` is omitted. The system prompt was already injected in the initial session; re-appending it on resume would duplicate instructions and waste context window.

And under **Key Decisions**, add a bullet:

- **Skip system prompt on resume.** The `--append-system-prompt-file` flag is only passed for new sessions, not resumed ones. Re-injecting the system prompt on resume duplicates instructions already present in the conversation history, wasting context and potentially confusing the agent.
