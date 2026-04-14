---
title: "Execution Policies"
owners: [cryppadotta]
---

---
title: "Execution Policies"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/governance/NODE.md", "product/task-system/NODE.md", "engineering/backend/NODE.md"]
---

# Execution Policies

Execution policies define when agent actions require human sign-off before proceeding. This extends the V1 approval gates (hiring, CEO strategy) with a configurable, policy-driven governance layer.

## Key Decisions

### Sign-Off-Required Policy

The Board can designate certain agent actions or action categories as requiring explicit human approval before execution. Agents block until sign-off is granted or denied, consistent with the existing approval workflow pattern.

**Rationale:** The V1 governance model gated only hiring and CEO strategy approval. As agents gain more autonomous capabilities, the Board needs finer-grained control over which actions can proceed without review. Execution policies provide this without requiring per-action hardcoding.

### Policy-Driven Configuration

Policies are configured by the Board, not hardcoded. This allows governance rules to evolve without code changes. The server evaluates policies and enforces blocking, consistent with the 'Governance Enforced at Server Layer' decision.

### Audit Trail

Policy evaluations and sign-off decisions are logged in the activity trail, extending the existing 'Auditable Everything' guarantee.

## Open Questions

- Which execution actions should have sign-off-required policies enabled by default vs. opt-in?
- How do execution policies interact with budget hard-stops (e.g., does a budget override bypass an execution policy)?
- Policy inheritance: can managers set execution policies for their reports, or is this Board-only?
