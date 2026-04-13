---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Inline document diff rendering (622a8e4) introduces a new pattern for displaying issue document edits within the thread — not covered by the existing issue-document-freshness node which only covers revision currency, not diff display.
---
# Document Diff Rendering

How document edits (issue description changes) are displayed inline within the issue thread, so agents and humans can see what changed without navigating to a separate diff view.

## Key Decisions

### Inline Diff Display

When an issue document is edited (by an agent updating the description or a human revising acceptance criteria), the diff is rendered inline in the issue thread rather than requiring navigation to a separate view. This keeps the full edit history visible in the natural reading flow of the thread.

### Rendering Inlined into Thread Components

Document diff rendering was refactored to be inlined directly into the thread rendering pipeline rather than being a separate component or view. This reduces architectural indirection and ensures diffs follow the same rendering path as other thread content (comments, status changes, etc.).
