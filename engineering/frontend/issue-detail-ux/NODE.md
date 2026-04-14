---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the primary surface where agents and humans inspect and manage individual issues.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This reduces navigation overhead — agents and humans can see the full task decomposition without switching views.

### Image Attachments as Gallery Grid

Image attachments are rendered as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview of attached images while maintaining consistent sizing.

### Scroll-to-Bottom Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate modal or page.
