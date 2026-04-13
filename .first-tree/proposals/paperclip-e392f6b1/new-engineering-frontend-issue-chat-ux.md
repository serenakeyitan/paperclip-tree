---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: PR #3079 polishes issue comments into a chat-like UX using @assistant-ui/react, but the tree has no node capturing the issue chat UX pattern — the conversational comment model, assistant-ui integration, and the design decision to make issue threads feel like chat rather than a traditional comment list.
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
