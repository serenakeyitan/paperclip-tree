---
title: "Issue Detail UX"
owners: [googlarz]
---

# Issue Detail UX

Rendering and layout patterns for the issue detail view — the primary surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation depth and gives agents immediate visibility into child work items without extra clicks or API calls.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid instead of a linear list. This provides a compact, scannable view when issues accumulate multiple screenshots or design assets.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open to avoid overlap. This is a layout coordination concern between the thread view and the side panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail rather than in a separate diff view. This keeps revision context co-located with the discussion thread.
