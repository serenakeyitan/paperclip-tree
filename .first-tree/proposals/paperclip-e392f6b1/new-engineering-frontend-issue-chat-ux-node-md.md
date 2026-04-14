---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3079 introduces a chat-like UX paradigm for issue comments using @assistant-ui/react — a fundamentally different interaction model from the markdown-polish focus of the existing issue-thread-ux nodes, which cover rendering quality but not the conversational layout pattern or assistant-ui integration.
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

This node covers the frontend chat interaction pattern. Markdown polish and reference resolution are in [engineering/frontend/issue-thread-ux](../issue-thread-ux/NODE.md). The product-level communication model is in [product/task-system/issue-thread-ux](../../product/task-system/issue-thread-ux/NODE.md). Backend comment APIs are part of [engineering/backend](../backend/NODE.md).
