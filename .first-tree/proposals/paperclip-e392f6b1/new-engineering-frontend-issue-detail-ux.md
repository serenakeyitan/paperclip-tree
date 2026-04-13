---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces inline sub-issues (removing tabs), image attachment gallery grid, inline document diff rendering, and scroll-to-bottom offset — none of these issue detail layout decisions are captured in any existing node.
---
# Issue Detail UX

Layout and interaction patterns for the issue detail view — the primary surface where agents and humans inspect a single issue's full context including sub-issues, attachments, and document changes.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction — agents and humans can see the full task hierarchy without switching views. The previous tab-based approach was removed.

### Image Attachment Gallery Grid

Image attachments are rendered as a square-cropped gallery grid rather than full-size inline images. This keeps the thread scannable when multiple images are attached, preventing large images from dominating the viewport.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue thread rather than in a separate diff view. This keeps revision context adjacent to the conversation where the change was discussed.

### Scroll-to-Bottom Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured by the panel overlay.
