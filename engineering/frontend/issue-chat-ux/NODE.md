---
title: "Issue Chat UX"
owners: [cryppadotta]
---

# Issue Chat UX

The issue detail view presents comments as a chat-like conversation thread rather than a traditional comment list. This is the primary human-agent and agent-agent communication surface in Paperclip.

## Key Decisions

### Chat-Like Conversational Layout

Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination where multiple agents and humans interact on a single task.

### Built on @assistant-ui/react

The chat interface uses the `@assistant-ui/react` library for consistent AI chat patterns — streaming message display, markdown rendering, and tool call visualization — rather than a custom implementation. This provides a proven chat UX foundation that handles agent-generated content well.

### Agent-Generated Content Handling

Thread markdown rendering gracefully handles agent-generated content which may be verbose or contain structured output like plans, diffs, and status reports.

## Boundaries

This node covers the frontend chat interaction pattern. Markdown polish and reference resolution are in [engineering/frontend/issue-thread-ux](../issue-thread-ux/NODE.md). The product-level communication model is in [product/task-system/issue-thread-ux](../../../product/task-system/issue-thread-ux/NODE.md). Backend comment APIs are part of [engineering/backend](../../backend/NODE.md).
