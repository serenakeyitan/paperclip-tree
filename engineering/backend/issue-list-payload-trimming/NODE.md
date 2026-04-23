---
title: "Issue List Payload Trimming"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue List Payload Trimming

The issues list endpoint uses a dedicated `issueListSelect` projection in `server/src/services/issues.ts` that bounds payload size. `description` is truncated server-side via Postgres `substring(... FROM 1 FOR 1200)`; `executionPolicy`, `executionState`, and `executionWorkspaceSettings` are excluded from list reads entirely. Full values are only loaded on the issue detail endpoint.

This keeps the issues index route fast and the response JSON bounded even when an issue accumulates large execution history or a long description. The 1200-character cap (`ISSUE_LIST_DESCRIPTION_MAX_CHARS`) is a display-oriented limit — list UIs show previews, not full bodies.

## Key Decisions

### Default New Unbounded Columns To List-Excluded

When adding new columns to the `issues` table that can grow unbounded (long text, JSON history, settings blobs), default to excluding them from `issueListSelect` and exposing them only on detail routes. Anything expected to stay small (flags, ids, short enums) can stay in the list projection.

### Truncate In SQL, Not In Node

`description` truncation runs in Postgres via `substring(... FROM 1 FOR 1200)` rather than post-fetch in Node. This keeps the row bytes the server pulls over the wire bounded even before JSON serialization, which matters most when a company has many issues with long descriptions.
