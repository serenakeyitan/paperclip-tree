---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

## Overview

The issue detail view is the primary workspace where humans and agents collaborate on tasks. It follows a chat-like thread layout with structured panels for metadata, attachments, and sub-issues.

## Key Decisions

- **Inline sub-issues** — Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context. Sub-issues default to the parent's workspace.
- **Image attachment gallery grid** — Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.
- **Scroll-to-bottom button offset** — The scroll-to-bottom button offsets when the properties panel is open, preventing overlap with side panels.
- **Inline document diff rendering** — Document diffs are rendered inline within the thread rather than in a separate view.

## Inbox Customization

The inbox grid supports show/hide columns including an optional Parent Issue column. Parent issue and time-ago columns use narrow widths to maximize space for issue content.
