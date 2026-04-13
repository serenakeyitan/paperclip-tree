---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Image attachments now render as a square-cropped gallery grid (1b55474) — a new rendering pattern for visual content that no existing node documents.
---
# Attachment Gallery

Rendering pattern for image attachments in issue threads — how visual content is displayed without dominating the thread layout.

## Key Decisions

### Square-Cropped Gallery Grid

Image attachments render as square-cropped thumbnails in a gallery grid rather than full-width inline images. This keeps the issue thread scannable when agents attach multiple screenshots or diagrams during debugging and implementation. The grid layout scales to handle many attachments without pushing text content out of view.

### Thread Readability Over Image Fidelity

The gallery prioritizes thread readability over showing full image detail. Users can click through to see full-resolution images. This trade-off matters because agents frequently attach screenshots as evidence of their work, and a thread with five full-width screenshots becomes unusable.
