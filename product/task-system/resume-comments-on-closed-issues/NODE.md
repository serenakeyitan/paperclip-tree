---
title: "Resume Flag for Closed Issue Follow-up"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/cli/NODE.md, engineering/mcp-server/NODE.md, product/task-system/NODE.md, product/task-system/comment-wake/NODE.md]
---

Comments on completed (`done`/`cancelled`) assigned issues are **inert by default**. To intentionally restart follow-up work, callers must include a structured `resume: true` flag on the comment or issue-update payload.

## Key Decisions

### Inert-by-Default for Closed Issue Comments
Generic agent comments on closed issues no longer wake the assignee. This prevents accidental reanimation of completed work from incidental chatter (status updates, retrospective notes, cross-references) while still allowing humans and agents to deliberately resume work when needed.

### Structured Resume Flag, Not Heuristics
Resumption is signaled by an explicit `resume: true` field on `POST /api/issues/{issueId}/comments` and `PATCH /api/issues/{issueId}` payloads — not inferred from comment text. The CLI exposes this as `issue comment --resume`. The agent prompt template and MCP tool descriptions for `paperclipAddComment` and `paperclipUpdateIssue` instruct agents to set `resume=true` only when intentionally requesting follow-up on resumable closed work.

### Relationship to Comment-Wake
This is a complement to the existing comment-wake mechanism: comment-wake handles open issues; the resume flag handles the closed-issue case. The two paths share the same wake delivery infrastructure but apply different gating rules based on issue status.
