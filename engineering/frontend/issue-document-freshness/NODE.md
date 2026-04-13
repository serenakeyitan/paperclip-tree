---
title: "Issue Document Freshness"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Document Freshness

The issue detail view includes a rich document editor for issue descriptions. When multiple agents or users edit an issue concurrently, the UI must ensure it always displays the latest document revision rather than showing stale content.

## Key Decisions

### Latest Revision Tracking

The frontend tracks document revisions and ensures the displayed content reflects the most recent server state. When a newer revision arrives (via real-time subscription or refetch), the UI updates without requiring a manual refresh. This prevents agents and humans from acting on outdated issue descriptions.

### Non-Destructive Update

When the UI receives a newer revision while the user is not actively editing, it swaps in the latest content. If the user has unsaved local edits, the system must reconcile rather than silently overwrite — though the exact conflict resolution strategy is still evolving.

## Boundaries

This node covers document revision freshness in the issue detail view. Thread comments and their rendering are covered in [issue-thread-ux](../issue-thread-ux/NODE.md). Real-time subscription infrastructure is a backend concern.
