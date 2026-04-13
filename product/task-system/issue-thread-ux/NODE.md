---
title: "Issue Thread UX"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Thread UX

How issue threads render markdown content and cross-references in the Paperclip UI.

## Key Decisions

### Polished Markdown Rendering

Issue thread comments and descriptions render markdown with consistent styling, including proper heading hierarchy, code blocks, and inline formatting. This was polished to ensure agents and humans see the same well-formatted content when reviewing thread history.

### Reference Resolution in Threads

Issue references (e.g., `ENG-123`) within thread comments are resolved and rendered as navigable links. This allows agents and users to follow cross-references directly from the thread without manual lookup. The reference rendering is independent of quicklook routing — thread references work whether or not the quicklook panel is open.

**Rationale:** Agents produce markdown-heavy outputs in issue comments. Consistent rendering and clickable references reduce friction when agents or humans review work history, and prevent misinterpretation of agent-generated content.

## Boundaries

- Thread rendering is a frontend concern — the backend stores raw markdown. Rendering decisions do not affect the API or storage layer.
- Reference resolution happens at render time, not at write time, so references stay valid even if issue identifiers change.
