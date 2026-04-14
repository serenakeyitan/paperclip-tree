---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the full-page surface where agents and humans inspect a single issue's content, sub-issues, attachments, and properties.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation depth and lets agents see the full task breakdown without switching context.

### Image Attachment Gallery Grid

Image attachments are rendered as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues contain multiple screenshots or diagrams.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing overlap with the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer.
