---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits change the issue detail view significantly — sub-issues moved inline (c414790), image attachments displayed as gallery grid (1b55474), scroll-to-bottom offset for properties panel (bf3fba3), document diff inlined (622a8e4) — but no issue detail UX node exists.
---
# Issue Detail UX

Rendering and layout patterns for the issue detail view — the primary surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation friction for agents and humans reviewing task hierarchies, keeping the full context visible in one scroll.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues accumulate screenshots, diagrams, or other image evidence during agent work.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer. This keeps revision comparisons in context with the rest of the issue discussion.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing it from being obscured. This is a layout coordination concern between the thread view and the side panel.

---

## Boundaries

- **Issue thread UX** (comment rendering, markdown polish) is covered by `engineering/frontend/issue-thread-ux` and `product/task-system/issue-thread-ux`.
- **Issue document freshness** (revision tracking) is covered by `engineering/frontend/issue-document-freshness`.
