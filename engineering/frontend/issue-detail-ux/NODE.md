---
title: "Issue Detail UX"
owners: [devinfoley]
---

# Issue Detail UX

Rendering and layout patterns for the issue detail view — the primary surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This reduces navigation overhead — agents and humans see the full task decomposition in one scroll without switching contexts.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than as a linear list or full-size images. This keeps the issue detail view compact when multiple images are attached.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer. This keeps the revision history visible in context.

### Properties Panel Scroll Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured.
