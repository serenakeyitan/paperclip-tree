---
title: "Issue Thread UX"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Thread UX

How issue threads render markdown content and cross-references for both agents and human users.

## Key Decisions

### Markdown Polish in Threads

Issue thread comments and descriptions render full markdown with proper formatting. This includes code blocks, lists, headings, and inline formatting. The polish layer is independent of routing (e.g., quicklook panels vs. full-page views) to ensure consistent rendering regardless of navigation context.

### Reference Rendering

Issue references (e.g., links to other issues, PRs, or agents) within thread content are rendered as navigable references rather than raw text. This allows agents and humans to quickly follow context chains without leaving the thread view.

**Rationale:** Agents produce markdown-heavy output (code snippets, structured plans, status updates). Poor rendering makes agent work harder to review. Reference navigation is critical for agents that need to follow dependency chains documented in comments.
