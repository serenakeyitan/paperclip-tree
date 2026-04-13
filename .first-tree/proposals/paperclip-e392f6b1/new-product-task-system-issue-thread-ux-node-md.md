---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR introduces polished markdown rendering and reference handling in issue threads — a new UX concern not captured by any existing task-system node.
---
# Issue Thread UX

How issue threads render markdown content and cross-references for both agents and human users.

## Key Decisions

### Markdown Polish in Threads

Issue thread comments and descriptions render full markdown with proper formatting. This includes code blocks, lists, headings, and inline formatting. The polish layer is independent of routing (e.g., quicklook panels vs. full-page views) to ensure consistent rendering regardless of navigation context.

### Reference Rendering

Issue references (e.g., links to other issues, PRs, or agents) within thread content are rendered as navigable references rather than raw text. This allows agents and humans to quickly follow context chains without leaving the thread view.

**Rationale:** Agents produce markdown-heavy output (code snippets, structured plans, status updates). Poor rendering makes agent work harder to review. Reference navigation is critical for agents that need to follow dependency chains documented in comments.
