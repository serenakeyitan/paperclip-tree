---
title: "Issue Descendant Filter"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/database/NODE.md, engineering/frontend/issue-list-ux/NODE.md, product/task-system/NODE.md]
---

# Issue Descendant Filter

The issue list API supports a `descendantOf=<issueId>` filter that returns every issue in the descendant subtree of a given root issue, not just direct children.

## Key Decisions

### Recursive CTE Over Application-Level Traversal

The filter is implemented as a recursive SQL CTE inside `issueService.list` rather than a multi-round-trip walk in application code. The CTE seeds with all direct children of `descendantOf` within the same `companyId` and unions in deeper generations until exhaustion. This keeps the entire subtree query in one round trip, which matters for issue trees that can be many levels deep.

### Distinct From `parentId`

`parentId` already filters to direct children only. `descendantOf` is a separate filter, not a replacement — both can coexist on the same query. The naming makes the depth semantics explicit at the API layer (`/companies/:id/issues?descendantOf=...`).

### Company-Scoped at Every Recursion Level

The CTE applies `companyId = ?` at both the seed and recursive step. This preserves Paperclip's company isolation invariant even inside recursive traversal — there is no path through which a descendant query could cross company boundaries.

### UI Plumbing

`issuesApi.list` accepts `descendantOf` and serializes it into the query string; the UI uses it to render full descendant lists for a root issue.
