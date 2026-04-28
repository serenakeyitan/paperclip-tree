---
title: "Issue Chat Thread Virtualization & Perf Harness"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/issue-chat-composer-viewport/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/task-system/issue-thread-ux/NODE.md]
---

`IssueChatThread` virtualizes its message list once the row count crosses `VIRTUALIZED_THREAD_ROW_THRESHOLD`. Below the threshold, all rows render directly; above it, the thread switches to a virtualized renderer to keep scroll and edit interactions responsive on long histories.

## Perf Harness

A DEV-only route `/tests/perf/long-thread` (`IssueChatLongThreadPerf` page) is registered under board routes when `import.meta.env.DEV` is true. It loads the `issueChatLongThread*` fixtures (comments, events, linked runs, transcripts, agent map) and renders `IssueChatThread` against them so engineers can profile long-thread behavior locally. The route is excluded from production builds.

## Refactor Notes

Fields previously plumbed through `IssueChatMessageContext` such as `feedbackVoteByTargetId`, `activeRunIds`, `stoppingRunId`, and `interruptingQueuedRunId` were removed from the shared context in favor of more localized props/state to reduce re-renders during virtualization. `findLatestCommentMessageIndex` is exported for use by scroll/anchor logic and tests.
