---
title: "Issue Threads"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Threads

Thread messages on issues render full markdown with inline reference resolution. References like `PAP-1234` are parsed and rendered as navigable links to the referenced issue, agent, or artifact.

## Key Decisions

### Quicklook-Independent Rendering

Thread rendering is decoupled from quicklook routing. Thread messages display identically whether viewed inline, in a detail panel, or via quicklook. This prevents UI regressions where routing context leaks into content rendering.

### Markdown Edge Cases for Agent Content

Agent-generated thread messages often contain code blocks, nested lists, and unusual whitespace. The renderer handles these edge cases to avoid broken display from non-human authorship patterns.
