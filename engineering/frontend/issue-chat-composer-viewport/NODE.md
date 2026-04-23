---
title: "Issue Chat Composer Viewport & Stop Run"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/heartbeat-run-orchestration", "product/task-system/comment-wake", "product/task-system/issue-thread-ux"]
---

# Issue Chat Composer Viewport & Stop Run

The issue chat thread preserves the composer's viewport position across re-renders and exposes a stop-run control so users can interrupt an in-flight agent run from the chat surface.

## Key Decisions

### Capture/Restore Composer Viewport

`captureComposerViewportSnapshot` and `restoreComposerViewportSnapshot` in `ui/src/lib/issue-chat-scroll.ts` save the composer's viewport position before message-list re-renders and restore it after layout settles. `shouldPreserveComposerViewport` only enables restoration while the composer contains the active element, so the UI keeps the editing surface anchored during streaming updates without fighting intentional navigation.

### Stable Thread Message Identity

`stabilizeThreadMessages` produces a memoization-friendly message array keyed by stable message identity, reusing prior message objects when their content fingerprint has not changed. This prevents `@assistant-ui/react` from remounting thread rows on metadata-only updates, which reduces flicker and makes the composer viewport snapshot meaningful across live transcript updates.

### Stop Run From Chat

`canStopIssueChatRun` gates a Stop Run action for queued or running issue-linked runs directly from the chat transcript UI. The frontend sends comment mutations with `interrupt: true`, and the backend cancels the active heartbeat run before logging the comment and propagating the interrupted run id through wake and activity payloads. This complements the existing comment-wake and reopen flows by giving board users a direct interrupt path without leaving the issue chat surface.

## Boundaries

- This node covers the chat-surface UX and client-side rendering behavior.
- Heartbeat lifecycle and cancellation semantics remain owned by `engineering/backend/heartbeat-run-orchestration/`.
- Comment-triggered wake behavior remains owned by `product/task-system/comment-wake/`.

Source: PR #3678 (`[codex] Improve issue detail and issue-list UX`)
