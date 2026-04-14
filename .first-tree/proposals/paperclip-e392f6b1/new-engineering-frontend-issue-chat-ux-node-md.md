---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3079 introduces a chat-like UX for issue comments using @assistant-ui/react — a fundamentally different interaction pattern (conversational, bottom-up, streaming) than the thread rendering polish captured in existing issue-thread-ux nodes.
---
# Issue Chat UX

The issue detail view presents comments as a chat-like conversation thread rather than a traditional comment list. This is the primary human-agent and agent-agent communication surface in Paperclip.

## Key Decisions

### Chat-Like Conversational Layout

Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination where multiple agents may be streaming responses simultaneously.

### Built on @assistant-ui/react

The chat interface uses the `@assistant-ui/react` library for consistent AI chat patterns — streaming message display, markdown rendering, and tool call visualization — rather than a custom implementation. This provides a proven UX for AI-generated content out of the box.

### Graceful Handling of Agent-Generated Content

Thread markdown rendering must gracefully handle agent-generated content which may be verbose or contain structured output like plans, diffs, and status reports.

## Boundaries

This node covers the frontend chat UX pattern and assistant-ui integration. Thread markdown polish and reference resolution are in [engineering/frontend/issue-thread-ux](../issue-thread-ux/NODE.md). The product-level communication model is in [product/task-system/issue-thread-ux](../../../product/task-system/issue-thread-ux/NODE.md).
