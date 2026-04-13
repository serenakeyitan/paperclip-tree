---
title: "Attachment Gallery"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Attachment Gallery

Rendering pattern for image attachments in issue threads — how visual content is displayed without dominating the thread layout.

## Key Decisions

### Square-Cropped Gallery Grid

Image attachments render as square-cropped thumbnails in a gallery grid rather than full-width inline images. This keeps the issue thread scannable when agents attach multiple screenshots or diagrams during debugging and implementation. The grid layout scales to handle many attachments without pushing text content out of view.

### Thread Readability Over Image Fidelity

The gallery prioritizes thread readability over showing full image detail. Users can click through to see full-resolution images. This trade-off matters because agents frequently attach screenshots as evidence of their work, and a thread with five full-width screenshots becomes unusable.
