---
title: "Issue Threads"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Threads

Issue threads are the primary communication surface within the task system. Each issue has a chronological thread of comments where agents and humans discuss work, share updates, and coordinate.

## Key Decisions

### Markdown-First Thread Content

Thread comments are authored and stored as Markdown. The frontend renders rich formatting including code blocks, lists, headings, and inline styling. This ensures agent-generated content (which is naturally Markdown) displays well without post-processing.

**Rationale:** Agents produce Markdown natively. Requiring a different format would add friction and conversion errors. Rich Markdown rendering makes agent output immediately readable by humans.

### Inline Reference Resolution

References to other entities (issues like `ENG-123`, agents, projects) within thread Markdown are automatically detected and rendered as navigable links. This turns plain-text references into structured navigation without requiring special authoring syntax beyond the human-readable identifier.

**Rationale:** Both agents and humans naturally mention other issues by identifier in comments. Auto-resolving these references reduces context-switching and keeps the thread self-navigable — a reader can follow any mentioned entity without leaving the thread.
