---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3039 introduces configurable execution policies with sign-off-required approval gates — a new governance mechanism the tree listed as deferred under 'fine-grained per-action governance gates' but never captured as a node.
---
# Execution Policies

Execution policies define when agent actions require human sign-off before proceeding. This extends the V1 approval gates (hiring, CEO strategy) with a configurable policy layer that can gate specific execution actions.

## Decision

Introduce a **sign-off-required execution policy** that allows the Board to designate certain agent actions or action categories as requiring explicit human approval before execution. This moves beyond the two fixed V1 approval gates toward a flexible, policy-driven governance model.

## Rationale

The V1 governance model gated only hiring and CEO strategy approval. As agents gain more autonomous capabilities, the Board needs finer-grained control over which actions can proceed without review. Execution policies provide this without requiring per-action hardcoding — the Board configures policies, and the server enforces them.

## Relationship to Existing Governance

Execution policies are enforced at the server layer (consistent with the 'Governance Enforced at Server Layer' decision). They extend the existing approval workflow — agents block until sign-off is granted or denied, same as hiring and strategy approvals. The audit trail captures policy evaluations and sign-off decisions.

## Open Questions

- Which execution actions should have sign-off-required policies enabled by default vs. opt-in?
- How do execution policies interact with budget hard-stops (e.g., does a budget override bypass an execution policy)?
- Policy inheritance: can managers set execution policies for their reports, or is this Board-only?
