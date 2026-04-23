---
title: "Dependency-Blocked Interaction"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/issue-blockers/NODE.md", "product/task-system/comment-wake/NODE.md"]
---

# Dependency-Blocked Interaction

Blocked issues must stay idle on their deliverable work while blockers remain unresolved, but humans can still comment on them. Paperclip distinguishes these cases so agents triage the comment without pushing forward the blocked work.

## Key Decisions

### Idle Until Final Blocker Resolves

Paperclip does not create a queued heartbeat run for a blocked issue until the final blocker is done and the `issue_blockers_resolved` wake fires. This prevents speculative work on deliverables whose prerequisites are incomplete.

**Rationale:** Heartbeat runs cost tokens and can produce premature artifacts. Blocked issues are not merely "waiting" — the dependency signal means the deliverable is not yet actionable. Suppressing heartbeat runs keeps agent effort aligned with the block/unblock state defined in [Issue Blockers](../issue-blockers/NODE.md).

### Human Comment Wakes Are Scoped to the Comment

When a human comments on a dependency-blocked issue, the wake prompt sets `dependencyBlockedInteraction: true` and passes `unresolvedBlockerIssueIds` / `unresolvedBlockerSummaries` (id, identifier, title, status, priority) into the agent prompt. The default wake template instructs the agent to respond or triage the comment but not treat the blocked deliverable as unblocked.

**Rationale:** Comment wake (see [Comment Wake](../comment-wake/NODE.md)) is the primary interactive channel. Without scoping the interaction, a comment-driven wake on a blocked issue could be misread as a signal to resume deliverable work. The explicit flag + blocker context keeps the agent responsive to the human while respecting the dependency gate.

### Blocker Summaries Travel With the Wake

The wake payload carries a normalized list of unresolved blockers so the agent can explain the block without a separate lookup, and so the prompt renderer can inline the context.

**Rationale:** Fetching blocker state at prompt render time adds latency and extra tool calls. Bundling the normalized summaries keeps the interaction snappy and ensures the agent's explanation reflects the exact blocker set that triggered the interaction flag.

## Boundaries

- The mechanics of declaring and resolving blockers — including the `issue_blockers_resolved` wake — are owned by [Issue Blockers](../issue-blockers/NODE.md).
- The general comment-driven wake path and its efficiency considerations are owned by [Comment Wake](../comment-wake/NODE.md). This node adds the blocked-issue interaction rules on top of that base behavior.
