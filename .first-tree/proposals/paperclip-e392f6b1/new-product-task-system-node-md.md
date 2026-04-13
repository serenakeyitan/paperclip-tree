---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR PIP-002 fixes stale execution lock lifecycle — the existing 'No Automatic Recovery' and 'Atomic Task Checkout' sections may need updating if this PR introduces automatic stale lock detection/release, changing the recovery policy from purely manual to having some automated cleanup.
---
## Stale Execution Lock Lifecycle (PIP-002)

When an agent crashes, times out, or otherwise fails mid-task, its execution lock can become stale — the task remains in `in_progress` with no agent actively working on it. PIP-002 addresses the lifecycle of these stale locks.

### Lock Staleness Detection

Execution locks are considered stale when the owning agent has not sent a heartbeat or made progress within a configurable timeout window. The system surfaces stale locks rather than silently accumulating them.

### Lock Release Policy

Stale locks follow a defined release lifecycle rather than remaining indefinitely. This refines the original "No Automatic Recovery" decision — while task *reassignment* remains manual/explicit, lock *cleanup* (releasing the physical execution lock so the task can be re-claimed) can follow automated lifecycle rules.

**Rationale:** The distinction between lock release and task recovery is important. A stale lock prevents any agent from claiming the task, which is a liveness problem. Releasing the lock does not reassign the task — it simply makes the task available for explicit re-assignment by a manager agent or the board.

### Boundaries

- Lock lifecycle is an execution concern (when to release a stale lock). Task recovery is a governance concern (who should pick up abandoned work). This PR addresses the former, not the latter.
- The execution workspace tied to a stale lock may also need cleanup — see execution workspaces for workspace lifecycle.
