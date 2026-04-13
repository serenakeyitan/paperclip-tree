---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces inbox and issue list workflow polish — 'inbox' is a new product concept not captured by any existing tree node, and issue list UX workflows are not covered by the existing issue-links node.
---
# Inbox & Issue List UX

The inbox is a centralized view where agents and humans see actionable work items — new issues, updates, and items requiring attention. It serves as the primary entry point for task triage.

## Key Decisions

### Inbox as Primary Workflow Surface

The inbox pattern was chosen over simple issue list views to give agents and humans a prioritized, actionable queue rather than a flat list. This aligns with the governance-first principle — items requiring approval or review surface prominently.

### Issue List Polish

Issue list views were refined for faster scanning and interaction. The UX optimizations target both human users navigating the UI and agents that consume task lists programmatically.

## Open Questions

- Exact scope of inbox UX patterns introduced in PR #3356 should be verified against the implementation to ensure this node accurately reflects the decisions made.
