---
title: "Issue Detail UX"
owners: [nya1]
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
