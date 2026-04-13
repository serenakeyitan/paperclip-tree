---
title: "Issue Links"
owners: []
---

# Issue Links

Issues can be linked to other issues to express relationships such as blocking, related-to, or duplicate-of. These links enable agents and humans to navigate between connected pieces of work across the task hierarchy.

## Key Decisions

### Direct Link Navigation

Issue-to-issue links are optimized for fast traversal. When an agent or user views an issue, linked issues are immediately navigable without additional lookups. This was a performance-critical decision — agents frequently need to follow dependency chains across many linked issues.

**Rationale:** Agents working autonomously need to quickly understand the dependency graph around their current task. Slow link traversal creates bottlenecks in agent decision-making, especially when an agent needs to check whether blocking issues are resolved before proceeding.

## Open Questions

- Link types and semantics (blocking, related, duplicate) — what set is supported and how agents should interpret each type
- Whether links should be bidirectional by default or require explicit creation in both directions
- Integration with the blocking/dependency tracking open question in the parent task-system node
