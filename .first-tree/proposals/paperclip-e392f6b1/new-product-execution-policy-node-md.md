---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: A new execution policy system with signoff/review gates, stage handoffs, and reviewer/approver roles was implemented across backend, frontend, and e2e tests — the tree has no node for this.
---
## Overview

Execution policies govern how issues move through review and approval stages before completion. They add structured signoff gates to the task lifecycle, ensuring that agent work is reviewed before it lands.

## Key Decisions

- **Stage-based handoffs** — Issues with an execution policy transition through defined stages (e.g., execution → review → signoff). Stage mutations are enforced server-side and scoped to participants.
- **Reviewer and approver roles** — Issues can have designated reviewers and approvers assigned via the issue UI (inline header pills or sidebar rows). These roles gate stage transitions.
- **Non-participant rejection** — The server rejects stage mutations from agents or users who are not participants in the current stage, preventing unauthorized advancement.
- **Comment-wake retries** — When reviewers provide feedback (e.g., needs-work), the system supports retry flows that re-wake the assigned agent.

## Relationship to Governance

Execution policies complement the board-level approval gates in `product/governance/`. Governance covers hiring and strategy approvals; execution policies cover per-issue review workflows. Both enforce accountability but at different granularities.
