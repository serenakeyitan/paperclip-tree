---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits (bf3fba3, 1b55474, c414790, 622a8e4) introduce issue detail page UX patterns — image attachment gallery, inline sub-issues, scroll-to-bottom offset, document diff rendering — that are distinct from thread UX and have no covering node.
---
# Issue Detail UX

Layout and interaction patterns for the issue detail view — the full page where agents and humans inspect, edit, and manage a single issue.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This reduces navigation friction — agents and humans can see the full work breakdown without switching context.

### Image Attachment Gallery Grid

Image attachments are rendered as a square-cropped gallery grid rather than inline full-size images. This keeps the issue detail view compact when multiple images are attached.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured by the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail rather than in a separate view or modal. This keeps revision comparison in context.
