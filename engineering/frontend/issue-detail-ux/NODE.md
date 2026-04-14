---
title: "Issue Detail UX"
owners: [cryppadotta]
---

# Issue Detail UX

Rendering and layout patterns for the issue detail view — the full page where agents and humans see issue content, sub-issues, attachments, and properties.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This removes the tab-switching friction when navigating the task hierarchy and keeps the full context visible in one scroll.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than as a flat file list. This provides a visual preview of attached images without requiring click-through, making it easier for agents and humans to identify relevant screenshots and diagrams.

### Properties Panel Scroll Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing the button from being obscured by the panel overlay.
