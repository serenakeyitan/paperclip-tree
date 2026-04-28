---
title: "Issue Board Workflow Polish"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/issue-chat-composer-viewport/NODE.md, engineering/frontend/issue-list-nesting-and-server-search/NODE.md, engineering/frontend/issue-list-ux/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/task-system/issue-blockers/NODE.md, product/task-system/issue-references/NODE.md]
---

Cross-cutting UX conventions for the issue board surfaces — chat thread, properties panel, list filters, markdown rendering, and keyboard navigation — that were polished as a single coherent workflow.

## Key Decisions

### Workspace-Scoped Issue List Filter

The issue list accepts an `initialWorkspaces` prop and a `workspace` URL query parameter so callers can deep-link into a workspace-filtered view. The filter is applied through the company issues endpoint via a generic `workspaceId` filter (`/companies/:id/issues?workspaceId=...`), distinct from the existing `executionWorkspaceId` filter — `workspaceId` is the broader user-facing scope, `executionWorkspaceId` remains the narrower execution-side filter. When `initialWorkspaces` is supplied, the stored view state's `statuses` are cleared so the workspace scope wins over any persisted status filter.

### Blocker Notice Above the Composer

`IssueChatThread` accepts a `blockedBy: IssueRelationIssueSummary[]` prop and renders an `IssueBlockedNotice` (PauseCircle icon) directly above the composer when the issue has unresolved blockers. The notice links each blocker through `IssueLinkQuicklook` so users can navigate without leaving the chat. This makes the blocker state actionable in the surface where the user is most likely to try to send a message.

### Reopen-on-Reassignment Includes `blocked`

`shouldImplicitlyReopenComment` in `CommentThread` now resumes issues to todo when an agent is reassigned on issues whose status is `done`, `cancelled`, or `blocked` (previously only the first two). Blocked issues with a fresh agent assignment should re-enter the queue, not stay parked.

### Markdown External-Link & GitHub Treatment

`MarkdownBody` opens external HTTP(S) links in a new tab with `rel="noopener noreferrer"`, leaves relative/internal links in-tab with `rel="noreferrer"`, and prefixes any github.com link (both explicit markdown links and autolinks) with an inline GitHub lucide icon. This applies uniformly across issue thread, comments, and document rendering so GitHub references are visually identifiable at a glance.

### Keyboard Shortcuts: Arrow Keys & Group Collapse

The inbox shortcut sheet now documents arrow-key equivalents for `j`/`k` (move down/up) and adds `←`/`→` for collapsing/expanding the selected group. The cheatsheet is the canonical reference — keep it in sync when adding new bindings.

### Identity Avatar Alignment

`Identity` always uses `items-center` for avatar+label alignment regardless of size. The previous `items-baseline` + `-top-px` workaround for `xs` is removed; activity rows now pass `align-middle` rather than `align-baseline`. Future avatar-bearing rows should follow the centered pattern.
