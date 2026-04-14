---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR adds sub-issues inline display, scroll-to-bottom button offset for properties panel, and image attachment gallery grid — these are issue detail view layout decisions not captured in the existing Issue Thread UX node.
---
### Sub-Issues Displayed Inline

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. The dedicated sub-issues tab has been removed in favor of inline display, reducing navigation overhead for agents and humans reviewing task hierarchies.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button in the issue view is offset when the properties panel is open, preventing the button from being obscured by the side panel.

### Image Attachments as Gallery Grid

Image attachments are displayed as square-cropped thumbnails in a gallery grid layout rather than as inline full-size images. This keeps the issue view compact when multiple images are attached.
