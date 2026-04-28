---
title: "Sub-Issues Workflow Checklist"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/frontend/issue-list-ux/NODE.md, engineering/frontend/storybook/NODE.md, product/task-system/NODE.md, product/task-system/issue-blockers/NODE.md]
---

Sub-issues of an issue can be presented as an ordered workflow checklist rather than a flat sub-issue list. The checklist surfaces step numbers, dependency relationships between steps, the current step, and an aggregate progress summary so agents and humans can treat a parent issue as a multi-step workflow.

## Key Decisions

### Workflow Sort Defines Step Order

A dedicated `workflow-sort` utility (`ui/src/lib/workflow-sort.ts`) computes the canonical ordering of sub-issues for checklist display. The order is derived from blocker relationships and existing sub-issue ordering, producing stable hierarchical step numbers like `1`, `2`, `2.1`. Step numbers are rendered in a monospace muted style next to the issue identifier in the row.

### Dependency Chips and Current Step

Each checklist row can render dependency chips (showing which earlier steps block this one) and a `checklistCurrentStep` flag that marks the active step. These are passed through `IssueRow` as first-class props (`checklistStepNumber`, `checklistCurrentStep`, `checklistDependencyChips`, `checklistRowId`) so the checklist shares the same row component as other issue lists rather than forking the rendering path.

### Progress Summary Derived Client-Side

`issue-detail-subissues.ts` exposes `buildSubIssueProgressSummary` and `shouldRenderSubIssueProgressSummary` to compute a checklist's completion state from the loaded sub-issue set. The summary is rendered above the list when the parent issue has enough sub-issues to make a checklist meaningful.

### Server Surfaces `blockedBy` for List Rendering

To render dependency chips without N+1 lookups, the issue list endpoint accepts `includeBlockedBy=true`. The service performs a single batched pass (`blockedByMapForIssues` in `server/src/services/issues.ts`) that returns blocker summaries (identifier, title, status, priority, assignee) for every issue in the list. The frontend opts in for the sub-issue checklist view.

### Storybook + Screenshot Coverage

The checklist has a dedicated Storybook story (`sub-issues-workflow.stories.tsx`) and a Playwright screenshot script (`scripts/screenshot-subissues.mjs`) that captures desktop and mobile, light and dark variants. Visual regressions on this surface should be caught by re-running the screenshot script against the story.
