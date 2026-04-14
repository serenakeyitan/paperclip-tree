---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the full-page surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation depth and gives immediate visibility into the breakdown of work.

### Image Attachment Gallery Grid

Image attachments are rendered as a square-cropped gallery grid instead of a linear list. This makes visual content scannable without dominating the vertical space of the issue view.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being hidden behind the panel.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than requiring navigation to a separate diff page.
