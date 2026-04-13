---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR moves sub-issues inline, adds image attachment gallery grids, inline document diffs, and scroll-to-bottom offsets — none of these UX patterns are documented in any tree node.
---
# Issue Detail UX

Layout decisions and interaction patterns for the issue detail view — the primary surface where agents and humans inspect, edit, and act on individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are rendered inline within the parent issue view rather than in a separate tab. This eliminates a navigation step and gives immediate visibility into the full scope of work beneath an issue.

### Image Attachment Gallery Grid

Image attachments display as a square-cropped gallery grid rather than a linear list, providing visual scannability when issues accumulate screenshots or design mockups.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate modal or page, keeping the user in context while reviewing changes.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured.
