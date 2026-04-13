---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3079 polishes the issue chat UX (chat-like comment threads using @assistant-ui/react), but the frontend node only mentions 'chat UX' in a single bullet — no dedicated node captures the conversational thread pattern, assistant-ui integration, or the design decisions around bottom-up message flow and streaming support.
---
# Issue Chat UX

The issue detail view presents comments as a chat-like conversation thread rather than a traditional comment list. This is the primary human-agent and agent-agent communication surface in Paperclip.

## Design Pattern

Issue comments are rendered using `@assistant-ui/react` components, giving the thread a chat-like feel with message bubbles, streaming support, and conversational flow. This aligns with the product decision that tasks are the communication channel — the chat UX makes that channel feel natural rather than formal.

## Key Decisions

- **Chat-like, not forum-like.** Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination.
- **Built on @assistant-ui/react.** The chat interface uses the assistant-ui library for consistent AI chat patterns (streaming, markdown rendering, tool call display) rather than a custom implementation.
- **Thread markdown rendering must gracefully handle agent-generated content** which may be verbose or contain structured output like plans, diffs, and status reports.
- **Issue reference parsing** follows the `{TEAM_KEY}-{NUMBER}` identifier format, rendering cross-references as navigable links within the chat.

## Boundaries

This node covers the frontend chat UX pattern. The task system's communication model (why tasks are the channel) is documented in [product/task-system](../../product/task-system/NODE.md). Backend comment APIs are part of [engineering/backend](../backend/NODE.md).
