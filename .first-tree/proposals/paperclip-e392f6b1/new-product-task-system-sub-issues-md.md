---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Sub-issues received significant UI and backend work — inline rendering, parent-child nesting in inbox, default workspace inheritance, parent issue columns — but the tree only mentions sub-issues as a hierarchy level without capturing the UX decisions.
---
## Sub-Issue UX and Hierarchy

Sub-issues are rendered inline on the parent issue page rather than in a separate tab. This keeps the parent issue as the single context for all child work.

### Key Decisions

- **Inline rendering.** Sub-issues appear directly on the parent issue detail page. A dedicated sub-issues tab was tried and removed in favor of inline display.
- **Workspace inheritance.** New sub-issues default to the parent issue's workspace, reducing friction when creating child tasks.
- **Inbox nesting.** The inbox/mine view nests child issues under their parent with a toggle button. Keyboard shortcuts (j/k) traverse nested children. Nesting is disabled on mobile for space reasons.
- **Parent issue column.** An optional Parent Issue column can be shown/hidden in the inbox grid for cross-referencing.

### Soft Links

- [Task System](NODE.md) — sub-issues are a core part of the task hierarchy.
- [Frontend UI](../../engineering/frontend/NODE.md) — inbox nesting and inline rendering are frontend UX patterns.
