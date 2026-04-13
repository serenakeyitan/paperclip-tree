---
title: "Execution Policies"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Policies

Execution policies define when agent actions require human sign-off before proceeding. This extends the V1 approval gates (hiring, CEO strategy) with a configurable, policy-driven governance layer.

## Decision

Introduce a **sign-off-required execution policy** that allows the Board to designate certain agent actions or action categories as requiring explicit human approval before execution. Rather than hardcoding new gates, the Board configures policies and the server enforces them at runtime.

**Rationale:** The V1 governance model gated only hiring and CEO strategy approval. As agents gain more autonomous capabilities, the Board needs finer-grained control over which actions proceed without review. Execution policies provide this flexibility without per-action hardcoding.

## Relationship to Existing Governance

Execution policies are enforced at the server layer, consistent with the existing 'Governance Enforced at Server Layer' decision. They extend the existing approval workflow — agents block until sign-off is granted or denied, same as hiring and strategy approvals. The audit trail captures policy evaluations and sign-off decisions.

## Open Questions

- Which execution actions should have sign-off-required policies enabled by default vs. opt-in?
- How do execution policies interact with budget hard-stops (e.g., does a budget override bypass an execution policy)?
- Policy inheritance: can managers set execution policies for their reports, or is this Board-only?
