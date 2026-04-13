---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

The issue detail view is the primary workspace where humans and agents collaborate on tasks. It follows a chat-like thread layout with structured panels for metadata, attachments, and sub-issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context. Sub-issues default to the parent's workspace.

**Rationale:** Agents frequently traverse parent-child relationships during execution. A separate tab forces extra navigation that breaks flow. Inline display keeps the full context visible.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate view, keeping context together and reducing navigation for review workflows.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets when the properties panel is open, preventing overlap with side panels.

## Boundaries

This node covers issue detail frontend UX patterns. The task data model is in [product/task-system](../../product/task-system/NODE.md). The chat/thread rendering pattern is in [issue-chat-ux](../issue-chat-ux/NODE.md). Backend APIs are in [engineering/backend](../backend/NODE.md).
