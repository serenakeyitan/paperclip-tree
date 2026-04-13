---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and component organization for the issue detail view — how sub-issues, attachments, and navigation elements are arranged within the issue page.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. Agents and humans navigating issue hierarchies need to see sub-issue status without switching context. The inline display reduces navigation friction for the most common workflow: reviewing a parent issue and its breakdown simultaneously. Sub-issues default to the parent's workspace.

### Image Attachments as Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button position accounts for the properties panel width when the panel is open, preventing the button from being obscured by side panels.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate view.

## Boundaries

This node covers the detail view layout. Chat UX patterns are in [engineering/frontend/issue-chat-ux](../issue-chat-ux/NODE.md). Thread markdown rendering is in [engineering/frontend/issue-thread-ux](../issue-thread-ux/NODE.md).
