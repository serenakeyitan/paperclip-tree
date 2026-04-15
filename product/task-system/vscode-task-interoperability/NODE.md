---
title: "VSCode Task Interoperability"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# VSCode Task Interoperability

Plan for integrating Paperclip's task system with VSCode's task runner, enabling developers to interact with Paperclip issues and agent work directly from their IDE.

## Status

Planning phase. A design plan has been drafted (see `doc/plans/2026-04-12-vscode-task-interoperability-plan.md` in source) but implementation has not begun.

## Context

This integration would allow VSCode users to view, assign, and track Paperclip issues without leaving the editor, bridging the gap between the Paperclip web UI and the developer's local workflow. The plan explores how Paperclip tasks map to VSCode's task provider API.

## Boundaries

- The task system domain model is owned by `product/task-system/NODE.md`.
- IDE-specific adapter behavior (e.g., Cursor) is owned by the respective adapter node.
