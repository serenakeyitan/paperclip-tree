---
title: "Issue Threads"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Threads

Issue threads are the primary conversation surface for tasks. Each issue has a thread of comments rendered as markdown, supporting inline issue references (e.g., `PAP-123`) that resolve to navigable links.

## Key Decisions

### Markdown rendering with issue references

Thread comments are rendered as full markdown with an additional pass that detects issue identifiers and converts them to interactive references. This lets agents and humans cross-reference related work inline without manual link construction.

### Thread rendering independent of quicklook routing

The thread markdown rendering pipeline is decoupled from the quicklook (hover preview) routing system. Thread polish — formatting, reference resolution, styling — operates independently so that changes to quicklook behavior (e.g., popover triggers, preview content) do not regress the base thread display. This separation was introduced explicitly after a regression where quicklook routing changes broke thread rendering.
