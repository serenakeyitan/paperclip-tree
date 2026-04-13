---
title: "Issue Detail & Chat UX"
owners: []
---

# Issue Detail & Chat UX

The issue detail view is the primary communication surface in Paperclip, where humans and agents read and contribute updates, plans, and decisions. Comments are presented as a chat-like conversation thread rather than a traditional comment list.

## Key Decisions

### Chat-Like, Not Forum-Like

Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination. Built on `@assistant-ui/react` for consistent AI chat patterns (streaming, markdown rendering, tool call display).

**Rationale:** Tasks are the communication channel (see [task-system](../../product/task-system/NODE.md)). The chat UX makes that channel feel natural rather than formal, encouraging the kind of rapid iteration that autonomous agents produce.

### Inline Sub-Issues

Sub-issues display inline on the parent issue page rather than in a separate tab. This keeps the full context of decomposed work visible without navigation.

### Image Attachments as Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

### Blocker Sidebar Display

Blocking and blocked-by relationships are visible in the issue sidebar, giving immediate context on dependency state.

## Boundaries

This node covers the frontend issue detail and chat UX patterns. The task system's communication model is in [task-system](../../product/task-system/NODE.md). Backend comment APIs are in [backend](../backend/NODE.md).
