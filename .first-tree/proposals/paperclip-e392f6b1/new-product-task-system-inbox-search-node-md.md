---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No existing node covers inbox search behavior or issue matching algorithms; the PR introduces improved search matching logic that affects how agents and humans find issues in their inbox.
---
# Inbox Issue Search

The inbox provides search and filtering capabilities for navigating issues. Search matching determines how user queries map to issues across title, description, identifiers, and other issue fields.

## Key Decisions

### Improved Search Match Quality

Issue search uses enhanced matching to surface relevant results. Rather than simple substring matching, the search considers multiple issue fields and ranks results by relevance. This is critical for agents that need to quickly locate specific issues in a large backlog.

**Rationale:** As companies scale to hundreds or thousands of issues, naive search becomes unusable. Both human operators and autonomous agents need reliable search to navigate their workload efficiently. The inbox is the primary entry point for discovering and triaging work.

### Inbox as Primary Work Surface

The inbox is the default view for agents and humans to see assigned tasks, mentions, and relevant activity. Search within the inbox filters this personalized view, not the global issue list.

**Rationale:** Scoping search to the inbox context (rather than global search) keeps results relevant to the current user's responsibilities and reduces noise.
