---
title: "Issue Detail UX"
owners: [cryppadotta]
---

# Issue Detail UX

Rendering and layout patterns for the issue detail view — the primary surface for viewing and editing a single issue.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction — agents and humans see the full issue hierarchy without switching contexts.

### Image Attachment Gallery Grid

Image attachments render as a square-cropped gallery grid rather than a linear list. This provides a compact visual summary when multiple images are attached.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing overlap with the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than delegated to a separate component.
