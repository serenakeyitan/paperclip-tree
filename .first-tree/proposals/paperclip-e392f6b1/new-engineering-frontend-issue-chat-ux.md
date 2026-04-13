---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The frontend node mentions 'chat UX' in a one-line bullet but no tree node explains the chat-like conversational pattern for issue threads, the @assistant-ui/react integration, markdown thread rendering, or issue reference parsing — all core to this PR.
---
# Issue Chat UX

The issue detail view presents comments as a chat-like conversation thread rather than a traditional comment list. This is the primary human-agent and agent-agent communication surface in Paperclip.

## Key Decisions

### Chat-Like, Not Forum-Like

Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination.

**Rationale:** Agents naturally produce conversational output. A chat-like layout makes agent-to-agent and human-to-agent communication feel natural, reducing the cognitive gap between "reading a task" and "talking to an agent."

### Built on @assistant-ui/react

The chat interface uses the assistant-ui library for consistent AI chat patterns (streaming, markdown rendering, tool call display) rather than a custom implementation.

### Markdown Thread Rendering

Thread comments render full markdown with support for code blocks, tables, and inline formatting. The rendering pipeline handles both human-authored and agent-generated markdown, which often includes verbose structured output like plans, diffs, and status reports.

### Issue Reference Parsing

Issue identifiers (e.g., `ENG-123`) within thread content are recognized and rendered as navigable links, following the `{TEAM_KEY}-{NUMBER}` format established in the task system. This creates implicit traceability between related work items.

## Boundaries

This node covers the frontend chat UX pattern. The task system's communication model (why tasks are the channel) is documented in [product/task-system](../../product/task-system/NODE.md). Backend comment APIs are part of [engineering/backend](../backend/NODE.md).
