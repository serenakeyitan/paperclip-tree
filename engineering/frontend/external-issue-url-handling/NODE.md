---
title: "External Issue URL Handling in Markdown"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/frontend/issue-editor-reliability/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/task-system/issue-references/NODE.md]
---

How the frontend distinguishes internal Paperclip issue references from absolute issue URLs when rendering markdown, mention chips, and issue-reference links.

## Key Decisions

### Absolute http(s) URLs Are Always External

`parseIssuePathIdFromPath` and `parseIssueReferenceFromHref` in `ui/src/lib/issue-reference.ts` no longer attempt to extract an internal issue id from a full `http://` or `https://` URL. Any href starting with `http(s)://` returns `null` from these parsers, even when the URL points at a Paperclip-shaped `/PROJECT/issues/PROJECT-123` path. This preserves origin, port, and hash fragments (e.g. `#comment-<uuid>`) that would otherwise be lost when collapsing to an internal `/issues/:id` route.

Only bare identifiers (`pap-1271`), relative paths (`/PAP/issues/PAP-1180`, `/issues/PAP-123`), and `issue://` scheme links are normalized into internal links.

### Mention Chips Skip Absolute URLs

`parseMentionChipHref` in `ui/src/lib/mention-chips.ts` short-circuits and returns `null` for any href starting with `http(s)://`, so absolute URLs never render as internal mention chips. They flow through the standard markdown link path and render as external links with `target="_blank"` and an external-link icon.

### Why This Matters

Issue URLs from other Paperclip deployments (different host, different port, with comment hash anchors) were previously rewritten into the local app's internal route, silently dropping the origin and any anchor. The result was a broken link that looked like it pointed somewhere on the current host but actually targeted a different deployment. Treating any absolute URL as external preserves user intent and avoids cross-deployment leakage.
