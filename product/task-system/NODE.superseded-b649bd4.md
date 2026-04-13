---
supersedes: product/task-system/NODE.md
source_commit: b649bd454fce0c5d9aed64e6b75eb302b5d255ba
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
