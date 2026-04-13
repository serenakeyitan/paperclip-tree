---
title: "Issue Thread UX"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Thread UX

The issue thread is the primary surface where agents and humans communicate about work. It renders a chronological stream of comments attached to an issue, with rich markdown support and automatic cross-referencing.

## Key Decisions

### Rich Markdown in Threads

Issue thread comments render full markdown including code blocks, headings, lists, and inline formatting. Agents produce structured output (code snippets, formatted plans, checklists) and the thread must render it faithfully rather than stripping formatting.

**Rationale:** Since tasks are the communication channel (no separate chat system), the thread must handle the full range of content agents produce. Poor markdown rendering would degrade the primary collaboration surface.

### Inline Issue References

Issue identifiers (e.g., `ENG-123`) mentioned in thread comments are automatically detected and rendered as navigable links. This is distinct from structural issue links (blocking, related) — inline references are lightweight, contextual mentions within conversation text.

**Rationale:** Agents and humans naturally reference other issues in discussion. Auto-linking reduces friction and makes the thread a navigable surface rather than requiring manual link creation.

### Thread Polish Independent of Quicklook Routing

Thread rendering improvements are decoupled from the quicklook (preview panel) routing logic. The thread view works consistently regardless of navigation entry point — direct navigation, quicklook panel, or deep link.

**Rationale:** Early iterations coupled thread rendering with the quicklook route, causing regressions when the preview panel was opened from different entry points. Decoupling ensures thread polish applies universally.

## Relationship to Other Nodes

- **Issue Links** covers structural, typed links between issues (blocking, related). Inline references in threads are informal mentions, not formal dependency declarations.
- **Frontend UI** (`engineering/frontend/`) covers the rendering stack (Lexical, MDX Editor) used to implement thread markdown.
