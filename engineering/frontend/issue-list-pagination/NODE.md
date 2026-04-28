---
title: "Issue List Pagination & Infinite Scroll"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/frontend/issue-list-nesting-and-server-search/NODE.md, engineering/frontend/issue-list-ux/NODE.md]
---

The issues list supports offset-based pagination with scroll-triggered loading of additional pages.

## Server

The `GET /companies/:companyId/issues` route accepts an `offset` query parameter alongside `limit`. Offset must be a non-negative integer; invalid values return HTTP 400. The validated `offset` is computed after parameter validation (it defaults to `0` when absent) and is passed through to the issues service.

## Client API

`issuesApi.list` accepts an optional `offset` filter and serializes it into the query string. `offset` is set whenever it is defined (including `0`), unlike `limit` which only sets when truthy.

## IssuesList Behavior

`IssuesList` accepts `hasMoreIssues`, `isLoadingMoreIssues`, and `onLoadMoreIssues` props. It walks ancestor elements to locate the nearest scrollable container (overflow `auto`/`scroll`/`overlay`) and falls back to the document scroll. When the user scrolls within `ISSUE_SCROLL_LOAD_THRESHOLD_PX` (320px) of the bottom and another page is available, it invokes `onLoadMoreIssues`. The previous fixed render-batch delay (`ISSUE_ROW_RENDER_BATCH_DELAY_MS`) is removed in favor of this scroll-driven approach.
