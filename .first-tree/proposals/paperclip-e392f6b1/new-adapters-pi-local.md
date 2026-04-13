---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Pi adapter node explicitly says 'No quota reporting' under Boundaries, but the PR adds quota exhaustion → failed run classification — a new failure semantic that changes adapter behavior.
---
# Pi Quota Exhaustion Handling

When the Pi adapter detects that the upstream API has exhausted its quota, the adapter classifies the run as **failed** (terminal) rather than retrying or pausing.

## Key Decisions

### Quota Exhaustion Is a Terminal Failure

Quota exhaustion is treated as a hard failure, not a transient error. The adapter marks the heartbeat run as `failed` and surfaces the failure to the task system. This means quota issues require manual intervention (e.g., upgrading the plan or waiting for the quota reset window) rather than silently retrying.

**Why:** Retrying against a quota-exhausted API wastes compute and delays surfacing the real problem. Agents need to know immediately that they cannot proceed so they can escalate or context-switch.

### Alignment with Run Lifecycle

This classification integrates with the heartbeat run orchestration's existing failure states. A quota-exhausted run transitions to `failed` with a descriptive reason, visible in the task system and audit trail.
