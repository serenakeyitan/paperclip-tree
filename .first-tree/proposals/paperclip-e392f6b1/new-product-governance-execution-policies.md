---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3039 introduces configurable execution policies with sign-off-required approval gates — a new governance mechanism the tree explicitly listed as deferred under 'fine-grained per-action governance gates'.
---
# Execution Policies

Execution policies define when agent actions require human sign-off before proceeding. This extends the V1 approval gates (hiring, CEO strategy) with a configurable, policy-driven governance layer.

## Key Decisions

### Sign-Off-Required Policy Type

The Board can designate specific agent actions or action categories as requiring explicit human approval before execution. Agents block until sign-off is granted or denied, consistent with existing approval workflow semantics.

**Rationale:** The V1 governance model gated only hiring and CEO strategy approval. As agents gain more autonomous capabilities, the Board needs finer-grained control over which actions proceed without review. Execution policies provide this without per-action hardcoding — the Board configures policies and the server enforces them.

### Server-Enforced

Execution policies are evaluated and enforced at the server layer, consistent with the existing 'Governance Enforced at Server Layer' decision. The audit trail captures policy evaluations and sign-off decisions.

## Open Questions

- Which execution actions should have sign-off-required policies enabled by default vs. opt-in?
- How do execution policies interact with budget hard-stops (e.g., does a budget override bypass an execution policy)?
- Policy inheritance: can managers set execution policies for their reports, or is this Board-only?
