Reliability conventions for the issue editor stack — the inline editor, markdown renderer/editor, quicklook, and the detail-page cache/query layer that feeds them.

## Key Decisions

### Shared Issue Detail Cache as the Source of Truth

`ui/src/lib/issueDetailCache.ts` and `issueDetailQuery` centralize how issue detail data is read and invalidated. Components rendering issue content (InlineEditor, MarkdownBody, IssueLinkQuicklook, IssueDetail) consume through this cache rather than holding their own copies, so edits, comment reopens, and link previews all see a consistent view.

### Editor Components Must Be Tested for Reliability, Not Just Rendering

`InlineEditor.test.tsx`, `MarkdownBody.test.tsx`, `MarkdownEditor.test.tsx`, and `issueDetailQuery.test.tsx` are first-class reliability tests — not snapshot or smoke tests. They cover edit lifecycle, cache invalidation, and quicklook routing. Changes to the editor stack must extend these tests rather than bypassing them.

### IssueLinkQuicklook Reads Through the Cache

Quicklook previews reuse `issueDetailCache` so hovering an issue link does not issue redundant fetches and always reflects the latest revision of the linked issue. This ties quicklook freshness to the same guarantees as the main detail page.

## How to Apply

When touching the issue editor, markdown rendering, or link quicklook, route all reads through `issueDetailCache`/`issueDetailQuery`, extend the paired `*.test.tsx` files, and preserve the invariant that the editor, body, and quicklook never display divergent revisions of the same issue.
