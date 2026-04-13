---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: A major new assistant-ui chat thread UX for issues was built — chat-style comments, chain-of-thought rendering, run transcripts, shimmer animations, and a reusable run feed on the dashboard — none of which the tree captures.
---
## Overview

Issue comments and agent run transcripts are rendered as a chat-like thread using the assistant-ui library. This replaces the traditional comment list with a conversational UX that makes agent interactions feel like messaging.

## Key Decisions

- **assistant-ui integration** — The chat thread is built on assistant-ui, which provides the message rendering primitives. Agent chain-of-thought output is mapped into assistant-ui message types.
- **Run transcript presentation** — Agent runs appear inline in the chat thread with shimmer animations for in-progress work (Working.../Worked tokens). Interrupted and non-succeeded runs are always rendered with their transcript.
- **Reusable run feed** — The chat-style run feed component is shared between the issue detail page and the dashboard, ensuring consistent presentation.
- **Crash guarding** — The issue chat is wrapped with error boundaries to guard against assistant-ui crashes, preventing a single rendering failure from breaking the entire issue detail view.
- **Inline composer** — The comment composer is inline within the chat thread with a capped height, stays visible while typing, and supports keyboard shortcuts (g c to focus).
