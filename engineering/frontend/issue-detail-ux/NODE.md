---
title: "Issue Detail UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail UX

The issue detail view is the primary workspace where humans and agents collaborate on tasks. It follows a chat-like thread layout with structured panels for metadata, attachments, and sub-issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context. Sub-issues default to the parent's workspace.

**Rationale:** Agents frequently create sub-issue hierarchies. Requiring tab navigation to see child issues breaks the workflow and hides important context from both agents and human reviewers.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

**Rationale:** Agents often attach multiple screenshots per task. Full-size inline images push thread content out of view, making it hard to follow the conversation.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate view, keeping context together.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets when the properties panel is open, preventing overlap with side panels.
