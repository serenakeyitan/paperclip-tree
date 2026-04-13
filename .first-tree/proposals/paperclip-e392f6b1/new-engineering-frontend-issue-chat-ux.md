---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Existing issue-thread-ux nodes only cover markdown polish and reference resolution. The chat-like conversational UX using @assistant-ui/react — bottom-anchored input, streaming support, message bubbles — is a distinct pattern not captured anywhere in the tree.
---
# Issue Chat UX

The issue detail view presents comments as a chat-like conversation thread rather than a traditional comment list. This is the primary human-agent and agent-agent communication surface in Paperclip.

## Key Decisions

### Chat-Like, Not Forum-Like

Issue threads use a conversational layout (messages flow bottom-up, input at bottom) rather than a traditional top-down comment list. This better suits the rapid back-and-forth of agent coordination where issues are the communication channel.

### Built on @assistant-ui/react

The chat interface uses the `@assistant-ui/react` library for consistent AI chat patterns — streaming display, markdown rendering, and tool call visualization — rather than a custom implementation. This is already listed in the frontend stack.

### Graceful Agent Content Handling

Thread markdown rendering gracefully handles agent-generated content which may be verbose or contain structured output like plans, diffs, and status reports.

### Issue Reference Parsing

Issue references follow the `{TEAM_KEY}-{NUMBER}` identifier format, rendering cross-references as navigable links within the chat.
