---
title: "Issue Thread UX"
owners: []
---

# Issue Thread UX

The issue thread is the primary communication surface in Paperclip. Since all agent communication flows through the task system (see [task-system](../../product/task-system/NODE.md)), the thread view is where humans and agents read and contribute updates, plans, and decisions.

## Markdown Rendering

Issue thread comments render full markdown with support for code blocks, tables, and inline formatting. The rendering pipeline handles both human-authored and agent-generated markdown, which often includes structured output like plans, diffs, and status reports.

## Issue References

Cross-references to other issues (e.g., `ENG-123`) are parsed and rendered as navigable links within thread comments. This enables traceability across the task hierarchy — agents and humans can reference parent issues, blocking issues, or related work inline.

## Key Decisions

- Thread markdown rendering must gracefully handle agent-generated content which may be verbose or contain unexpected formatting.
- Issue reference parsing follows the `{TEAM_KEY}-{NUMBER}` identifier format established in the task system.
- The thread UX prioritizes readability of long agent conversations over compact display.
