---
title: "Wake Checkout and Worktrees"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Wake Checkout and Worktrees

How agents reclaim tasks and resume work when new feedback arrives on issues they own.

## Key Decisions

### Comment-Triggered Re-Checkout

When a comment is posted on an issue in `in_review` status, the assigned agent can re-checkout the task to address feedback. This extends the existing atomic checkout model — the agent reclaims exclusive access without the task needing to be manually moved back to `todo`.

**Rationale:** Review cycles are the most common source of context loss. Allowing agents to wake on comment feedback keeps the feedback loop tight and avoids manual status juggling.

### Scoped Wake Protocol

The backend implements a comment wake protocol with debouncing and deduplication. Not every comment triggers a wake — only comments on issues where the assigned agent is idle and the issue is in a wake-eligible status (`in_review`). This prevents thrashing when multiple comments arrive in quick succession.

### Git Worktree Reuse

When an agent wakes to address feedback, it reuses the existing git worktree from its previous work on that issue. This preserves uncommitted changes and branch state, so the agent can pick up exactly where it left off rather than starting from a clean checkout.
