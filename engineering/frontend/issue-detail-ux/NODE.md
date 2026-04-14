---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the full-page surface where agents and humans inspect, edit, and navigate individual issues.

## Key Decisions

### Inline Sub-Issues (No Tab)

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This reduces navigation depth and lets agents see the full issue hierarchy at a glance without switching views.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues contain multiple screenshots or diagrams.

### Scroll-to-Bottom Offset with Properties Panel

The scroll-to-bottom button is offset when the properties panel is open to avoid overlap. This is a layout coordination concern between the thread scroll area and the side panel.
