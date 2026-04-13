---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

Layout and rendering decisions for the issue detail view — the primary workspace for understanding and acting on a single issue.

## Key Decisions

### Inline Sub-Issues (No Tabs)

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This keeps the full task hierarchy visible in context, reducing navigation hops for agents that need to understand parent-child relationships while working on an issue.

### Square-Cropped Image Gallery Grid

Image attachments render as a square-cropped gallery grid rather than full-size inline images. This prevents large images from dominating the conversation thread and allows quick visual scanning of multiple attachments.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button shifts horizontally when the properties panel is open to avoid being obscured. Layout-aware positioning ensures interactive controls remain accessible regardless of panel state.

### Inline Document Diff Rendering

Document diffs render inline within the conversation thread rather than in a separate view. This keeps change context co-located with the discussion, matching the conversational workflow agents and humans use when reviewing changes.
