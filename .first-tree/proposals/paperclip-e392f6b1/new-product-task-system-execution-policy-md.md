---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: A major new feature — execution policies with signoff/review gates, stage handoffs, and reviewer/approver assignment — was added across backend, UI, and tests, but the tree has no node capturing this decision or its workflow model.
---
## Execution Policy

Execution policies define the workflow stages an issue must pass through before completion. A policy can require reviewer sign-off, approver gates, or multi-stage handoffs between agents and humans.

### Key Decisions

- **Stage-based model.** Issues with an execution policy progress through ordered stages (e.g. draft → in-progress → in-review → approved). The server enforces that only the designated participant for each stage can mutate it forward.
- **Reviewer and approver assignment.** Issues can have explicit reviewer and approver fields, set at creation time or later. These are surfaced in the issue sidebar and new-issue dialog.
- **Server-enforced access.** Non-participant stage mutations are rejected by the backend. This prevents agents from bypassing human review gates.
- **Comment-wake retries.** When a reviewer leaves feedback (e.g. needs-work), the system wakes the assigned agent to retry, respecting the execution policy's stage rules.

### Soft Links

- [Governance](../governance/NODE.md) — execution policies are an extension of the governance approval model.
- [Task System](NODE.md) — execution policies attach to the existing issue lifecycle.
