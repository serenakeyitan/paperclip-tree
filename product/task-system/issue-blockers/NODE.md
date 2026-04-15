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

## Relationship to Issue Links

Blocker relationships build on the issue link infrastructure but add behavioral semantics. The drift node at `drift/paperclip-e392f6b1/product/task-system/issue-links/` captured the open questions around link types — this node resolves the blocker/dependency subset of those questions.
