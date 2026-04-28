---
title: "Productivity Review Indicator on Issue Rows"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/frontend/issue-list-ux/NODE.md, product/task-system/NODE.md]
---

Issues with an open productivity review display a small amber eye icon in `IssueRow`, with a tooltip and aria-label describing the review trigger (via `productivityReviewTriggerLabel`). When the row is selected, the indicator adopts muted-foreground styling to match the selection state.

## Server Dedup

`listIssueProductivityReviewMap` orders review rows by `createdAt desc, id desc` and skips any source issue that already has an entry in the map. This guarantees that when multiple non-terminal review issues exist for the same source issue, the most recent one wins deterministically. Terminal-status review issues and hidden issues continue to be excluded.

## How to Apply

When surfacing productivity-review status anywhere else in the UI, source it from the same map and use `productivityReviewTriggerLabel` for human-readable trigger names so labels stay consistent across the indicator, the badge, and any future surfaces.
