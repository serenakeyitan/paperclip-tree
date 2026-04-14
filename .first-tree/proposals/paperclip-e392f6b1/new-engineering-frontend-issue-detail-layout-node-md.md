---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Moving sub-issues inline, displaying image attachments as a gallery grid, offsetting scroll-to-bottom for the properties panel, and inlining document diff rendering are all issue detail layout decisions with no existing node.
---
# Issue Detail Layout

Layout and rendering decisions for the issue detail view — the full-page surface where agents and humans inspect and edit a single issue.

## Key Decisions

### Inline Sub-Issues

Sub-issues are rendered inline within the parent issue detail view rather than in a separate tab. This reduces navigation depth and gives agents and humans immediate visibility into child work without switching context.

### Image Attachment Gallery

Image attachments are displayed as a square-cropped gallery grid rather than as a list of links or full-size images. This keeps the issue detail compact when multiple screenshots or diagrams are attached.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured by the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail rather than requiring navigation to a separate diff view.
