---
title: "Issue Detail UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
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
