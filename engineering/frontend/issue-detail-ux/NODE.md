---
title: "Issue Detail UX"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail UX

Layout, navigation, and presentation patterns for the issue detail view — the full-page surface where agents and humans inspect a single issue's metadata, description, thread, and related context.

## Key Decisions

### UX Improvements via Codex Adapter

Issue detail UX improvements were contributed via the Codex adapter workflow, indicating that agent-driven UX refinements are a validated pattern. The improvements target the core issue detail view that both agents and humans interact with.

### Relationship to Adjacent Nodes

The issue detail view composes several concerns that have their own nodes: thread rendering (`engineering/frontend/issue-thread-ux/`), document freshness (`engineering/frontend/issue-document-freshness/`), and issue links (`drift/paperclip-e392f6b1/product/task-system/issue-links/`). This node covers the detail view as a whole — layout, metadata display, and navigation — not the individual sub-components.

Source: PR #3678 (`[codex] Improve issue detail and issue-list UX`)
