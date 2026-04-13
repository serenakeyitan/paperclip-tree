---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Issue comments were redesigned into a chat-like UX using assistant-ui, with chain-of-thought rendering, shimmer animations for in-progress runs, and the chat thread reused on the dashboard — a major UX paradigm shift not captured in the tree.
---
## Issue Chat UX

Issue comments are rendered as a chat thread rather than a traditional comment list. This makes agent-human interaction feel conversational and surfaces agent work-in-progress inline.

### Key Decisions

- **assistant-ui thread model.** The issue detail page uses an assistant-ui chat thread component. Agent transcript messages, chain-of-thought, and human comments are unified into one stream.
- **Shimmer and working state.** When an agent run is in progress, a shimmer animation with a Working/Worked token indicates live activity. The shimmer loops with a pause between repeats to avoid visual jitter.
- **Chat-style comment rendering.** Comments use chat-bubble styling — user avatars on the right margin, agent messages left-aligned. Action bars with date and menu are overlaid.
- **Dashboard reuse.** The chat-style run feed is reused on the dashboard, giving a consistent view of agent activity across pages.
- **Crash guard.** The chat component is wrapped in an error boundary to prevent assistant-ui crashes from taking down the issue detail page.

### Soft Links

- [Frontend UI](../../engineering/frontend/NODE.md) — chat UX is a major frontend pattern.
- [Agent Model](../agent-model/NODE.md) — chain-of-thought and transcript rendering depend on the agent output model.
