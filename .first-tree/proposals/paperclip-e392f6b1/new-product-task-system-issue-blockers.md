---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing drift issue-links node lists blocker semantics as an open question, but this PR resolves it — blocking is now a first-class link type with defined behavioral side effects (status transitions, dependency wakeups). The tree needs to capture these resolved decisions.
---
# Issue Blockers

First-class blocking relationships between issues. When issue A blocks issue B, resolving A triggers behavioral side effects on B — including automatic agent wakeups and UI indicators.

## Key Decisions

### Blocking as a Distinguished Link Type

Among the supported issue link types (blocking, related, duplicate), `blocks`/`blocked_by` is the only link type with server-enforced behavioral side effects. Other link types are navigational; blocking links are operational. Resolving a blocker can trigger dependency wakeups and surface blocked issues in agent inboxes.

**Rationale:** Blocking relationships carry semantic weight that other link types don't — they affect whether an agent can make progress. Treating them as a special case at the server layer enables automated coordination (wakeups) without requiring agents to implement their own polling logic.

### Inline Visibility

Blocked/blocking relationships are shown inline on the issue detail view rather than in a separate tab. Sub-issues are also rendered inline. This reduces navigation friction for both agents querying the API and humans using the Board UI.

**Rationale:** Agents and humans need to see blocking context immediately when viewing an issue, not after navigating to a separate panel. Inline rendering reduces the steps to understand an issue's dependency state.

## Relationships

- Issue links (`product/task-system/issue-links`) define the link model; this node captures the blocker-specific semantics.
- Dependency wakeups (`product/task-system/dependency-wakeups`) are the primary behavioral side effect of blocker resolution.
- The task-system open question 'Blocking/dependency tracking between issues' is resolved by this feature.
