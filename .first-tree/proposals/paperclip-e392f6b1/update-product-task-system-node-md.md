---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: product/task-system/NODE.md
supersedes: product/task-system/NODE.md
rationale: The task system node lists sub-issues in the hierarchy but the source now has significant sub-issue features: inline display, parent issue columns, parent-child nesting in inbox, default workspace inheritance, and blocker/dependency wakeups — none captured in the tree.
---
## Sub-Issues and Blockers

Sub-issues are now first-class citizens in the issue detail and inbox views, with blocker tracking and dependency-driven wakeups.

### Sub-Issue Display

- Sub-issues are displayed inline on the parent issue page (not in a separate tab).
- Inbox supports parent-child nesting with a toggle button; keyboard shortcuts (j/k) traverse nested children.
- An optional "Parent Issue" column is available in inbox and issue list views.
- Sub-issues default to the parent issue's workspace.

### Blockers and Dependencies

- Issues can declare blockers (other issues that must complete first).
- Blocker resolution triggers dependency wakeups — blocked issues are automatically notified when their blockers are resolved.
- Blocker and review activity events are tracked in the activity log.

### Decisions

- Inline sub-issues over tabbed navigation — reduces clicks and keeps context visible.
- Parent-child nesting is disabled on mobile for layout simplicity.
- Blocker wakeups are event-driven, not polling-based.
