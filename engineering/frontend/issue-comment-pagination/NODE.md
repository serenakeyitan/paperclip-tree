---
title: "Issue Comment Pagination & Autoload"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/backend/NODE.md, engineering/frontend/issue-chat-composer-viewport/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/task-system/issue-thread-ux/NODE.md]
---

How issue comments are paged on the server and progressively loaded on the client to prevent comments from disappearing from the visible thread while keeping the initial render bounded.

## Key Decisions

### Anchor-Based Composite Cursor

The server pages issue comments around an anchor comment using a composite ordering of `(createdAt, id)`. For descending pagination (older comments), the predicate is `createdAt < anchor.createdAt OR (createdAt = anchor.createdAt AND id < anchor.id)`; ascending pagination mirrors the same shape with `>`. Using `id` as a tiebreaker guarantees a stable, total order even when multiple comments share the same `createdAt` timestamp, which is what previously caused comments to be skipped or duplicated as users paged.

The predicate is expressed via Drizzle's `gt`/`lt`/`and`/`or` builders rather than raw `sql<boolean>` templates so the bound parameters are typed and the conditions compose with the rest of the query's `WHERE` clause cleanly.

### Chat-Tab Autoload Policy

The frontend automatically pages older comments only when:

- the active detail tab is `chat`,
- there are older comments available,
- no initial or older page is currently loading,
- at least one comment has already loaded (i.e. the initial page resolved), and
- the total loaded comment count is below `ISSUE_COMMENT_AUTOLOAD_LIMIT` (currently `ISSUE_COMMENT_PAGE_SIZE * 3` = 150).

This policy lives in `shouldAutoloadOlderIssueComments` in `ui/src/lib/optimistic-issue-comments.ts` and is consumed by `IssueDetail.tsx`. It bounds the cost of opening a long thread while ensuring the chat surface fills enough context to feel continuous, and it deliberately does not autoload on non-chat tabs (e.g. activity) where the comment list is secondary.
