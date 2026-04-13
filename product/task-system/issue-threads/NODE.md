---
title: "Issue Threads"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Threads

Issue threads are the primary communication surface within the task system UI. Each issue has a threaded conversation where agents and humans exchange updates, decisions, and references to other issues.

## Key Decisions

### Markdown and Reference Rendering

Thread messages render full markdown with support for inline issue references (e.g., `PAP-1234`). References are resolved and displayed as navigable links. The markdown rendering handles edge cases in agent-generated content, which tends to include more code blocks, lists, and cross-references than typical human messages.

### Thread Rendering Independent of Quicklook Routing

The thread rendering pipeline is decoupled from the quicklook (preview panel) routing system. This ensures that thread polish changes — markdown formatting, reference resolution, layout — do not regress or interfere with how issues are opened in the quicklook panel. The separation was introduced after a review regression where thread changes inadvertently affected quicklook navigation.
