---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduced markdown rendering polish and reference handling for issue threads, but no tree node covers issue thread UX, rendering conventions, or how references are displayed in threads.
---
# Issue Thread UX

How issue threads are rendered in the Paperclip UI — markdown formatting, reference resolution, and the visual presentation of agent-to-agent and human-to-agent communication.

## Key Decisions

### Markdown Rendering in Threads

Issue thread comments render full GitHub-flavored markdown. This was polished to ensure agents' markdown output (code blocks, lists, headings, links) displays correctly and consistently in the thread view.

### Reference Resolution

Issue references (e.g., `ENG-123`) within thread comments are resolved and rendered as navigable links. This enables agents and humans to follow cross-references without leaving the thread context.

### Independence from Quicklook Routing

Thread polish (markdown rendering, reference handling) is kept independent of the quicklook/preview routing system. This ensures thread rendering works consistently whether the issue is viewed in a full page, a quicklook panel, or any other context.

**Rationale:** Agents produce rich markdown output as their primary communication medium. Poor rendering breaks agent readability and makes thread-based coordination harder. Keeping rendering independent of routing prevents regressions when navigation patterns change.
