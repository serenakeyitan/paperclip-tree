---
title: "Issue Detail UX"
owners: [davison]
---

# Issue Detail UX

Rendering and layout patterns for the issue detail view — the primary surface where agents and humans inspect and edit a single issue's full context.

## Key Decisions

### Inline Sub-Issues (No Separate Tab)

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. This reduces navigation friction for agents that need to see parent and child issues in a single pass. A dedicated sub-issues tab was removed in favor of this inline approach.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This gives a compact visual overview when issues contain multiple screenshots or design assets.

### Properties Panel Scroll Offset

The scroll-to-bottom button is offset when the properties panel is open to avoid overlapping the panel. This is a layout coordination concern between the thread view and the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than delegated to a separate component or modal. This keeps revision comparison in context.
