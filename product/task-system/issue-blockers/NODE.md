---
title: "Issue Blockers"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Blockers

First-class blocking relationships between issues, with automatic dependency wakeups when blockers are resolved.

## Key Decisions

### Blockers as a Distinct Relationship Type

Blocker links are not just another issue link type — they are a first-class concept with behavioral side effects. When issue A blocks issue B, callers are responsible for setting B's status explicitly — `blockerRelation` itself does not enforce or change status. The semantic weight of the link drives agent behavior (agents check for blockers before proceeding), not automated status transitions. This goes beyond the generic link navigation described in the issue links model.

**Rationale:** Agents need to coordinate work across dependency chains. A generic "related-to" link doesn't carry enough semantic weight to drive automated behavior. Making blockers first-class enables the system to automate dependency resolution workflows.

### Dependency Wakeups

When **all** of a dependent issue's blockers transition to `done`, the system fires a dependency wakeup for the dependent's assigned agent via the `blockedByIssueIds` relation. A `cancelled` blocker does not count as resolved — only `done` clears the dependency. There is no per-dependent opt-in flag; the wakeup fires automatically once the all-blockers-done condition is met.

**Rationale:** Without automatic wakeups, agents blocked on dependencies would need to poll or rely on manual intervention. Dependency wakeups close the loop — an agent that is blocked can trust that the system will wake it once every blocker is resolved. The all-blockers-done gate prevents premature wakeups when only some blockers are cleared. This is critical for efficient multi-agent coordination.

### Integration with Wake Protocol

Dependency wakeups use the same wake event infrastructure as other agent wake triggers (e.g., comment mentions, assignment changes). The wake event is scoped to the unblocked issue, enabling auto-checkout on wake to function correctly.

### Blocker Context Lives Next to the Composer

When an issue is blocked, the issue thread renders the unresolved blocker summary inline with the composer, not only in the properties panel. The composer is where the next action happens — a new comment, a handoff, a reassignment — so the blocker list is placed where it will actually inform that action. Putting it only in the properties panel would require agents and humans to deliberately scan for it before contributing, and many would not.

**Rationale:** The cost of a blocked contribution (a comment that ignores the blocker, a handoff to an agent who cannot make progress) is high. Surfacing blockers at the point of action is a small UX change with outsized coordination value.

### Issue Thread Treats Blocked as Resumable on Agent Reassignment

In the issue chat thread, a comment that reassigns to an agent is treated as an implicit reopen when the issue status is `done`, `cancelled`, **or** `blocked`. Previously only the terminal states (`done`, `cancelled`) triggered implicit reopen in the composer; `blocked` is now treated as a resumable state in this flow because a fresh agent assignment is a strong signal that the blocker condition should be retested rather than left dormant.

**Scope:** This is a UI / issue-thread decision, not a platform-level workflow invariant. The evidence in `paperclipai/paperclip#4224` is UI-side only — `ui/src/components/IssueChatThread.tsx` broadens `shouldImplicitlyReopenComment(...)`, and `ui/src/lib/optimistic-issue-comments.ts` broadens `applyOptimisticIssueCommentUpdate(...)` so the optimistic view reflects the same resume rule. The server-side task-system source of truth for whether reassignment of a blocked issue persistently reopens to `todo` is not established by that PR; if and when the server-side rule is added, this decision should be promoted and this node updated to cite that source.

**Rationale:** A blocked issue can stay blocked long after its blocker is effectively resolved, either because the dependency wakeup was missed or because the blocker was resolved out-of-band (e.g., a workaround, a scope change). When an operator reassigns a new agent from the issue thread, they are asserting that the issue is worth pursuing again — the right composer default is to put it back in `todo` optimistically and let the new agent re-evaluate the block, not to leave it dormant in the thread.

## Relationship to Issue Links

Blocker relationships build on the issue link infrastructure but add behavioral semantics. The drift node at `drift/paperclip-e392f6b1/product/task-system/issue-links/` captured the open questions around link types — this node resolves the blocker/dependency subset of those questions.
