---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and rendering decisions for the issue detail view — the full-page surface where agents and humans inspect, edit, and manage a single issue.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. The sub-issues tab was removed in favor of inline rendering, reducing navigation friction for agents that need to see the full issue hierarchy at a glance.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than as a linear list or raw links. This makes visual artifacts (screenshots, diagrams, generated images) immediately scannable without clicking through individual attachments.

### Document Diff Inline Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer. This keeps revision context visible alongside the rest of the issue without requiring navigation to a dedicated diff page.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured by the panel overlay.
