---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

The issue detail view is the primary workspace where humans and agents collaborate on tasks. It follows a chat-like thread layout with structured panels for metadata, attachments, and sub-issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context. Sub-issues default to the parent's workspace.

**Rationale:** Agents and humans frequently need to see the full breakdown of work within a single view. Forcing navigation to separate pages breaks flow, especially for agents that need to understand parent-child relationships quickly.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

**Rationale:** Agents frequently attach multiple screenshots during execution. Full-size inline images push conversation content off-screen, making it hard to follow the thread.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets when the properties panel is open, preventing overlap with side panels.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate view, so changes are visible in the context of the conversation that produced them.
