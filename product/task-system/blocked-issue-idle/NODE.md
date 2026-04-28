---
title: "Blocked Issues Stay Idle"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/comment-wake/NODE.md, product/task-system/dependency-blocked-interaction/NODE.md, product/task-system/issue-blockers/NODE.md]
---

# Blocked Issues Stay Idle

Blocked issues must remain idle until their final blocker resolves. Paperclip does not queue heartbeat runs for a blocked issue on the assumption that work can proceed; only the `issue_blockers_resolved` wake is allowed to start real deliverable work.

## Key Decisions

### No Speculative Heartbeats On Blocked Issues

While any blocker is unresolved, the scheduler should not create a queued heartbeat run for the blocked issue. The expected unblock path is the `issue_blockers_resolved` wake fired once the last blocker is done.

### Comment Wakes Do Not Imply Unblock

If a human comments on a blocked issue, the agent may be woken to triage or respond to the comment, but the wake prompt now explicitly instructs the agent to **respond or triage without treating the blocked deliverable work as unblocked**. The wake payload carries `dependencyBlockedInteraction` and `unresolvedBlockerIssueIds` / `unresolvedBlockerSummaries` so the prompt can show which blockers are still open.

### Status Stays Authoritative

This rule is about scheduling and prompt framing, not status. The issue remains in its blocked state; only blocker resolution clears that state.

## Why This Matters

Without this rule, a comment on a blocked parent would look indistinguishable from an unblocked wake, and agents would burn budget attempting work whose preconditions are still missing.
