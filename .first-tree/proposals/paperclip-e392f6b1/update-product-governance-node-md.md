---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: product/governance/NODE.md
supersedes: product/governance/NODE.md
rationale: Board approvals now appear on issue boards, authorization scoping was hardened for import/approvals/activity/heartbeat routes, and JWT secret handling was fixed — governance enforcement expanded beyond what the node describes.
---
The existing node should be updated to reflect:

- **Route-level authorization scoping** — Import, approvals, activity, and heartbeat routes are now explicitly scoped with authorization checks, closing gaps in the enforcement layer.
- **JWT secret hardening** — The hardcoded JWT secret fallback was removed from `createBetterAuthInstance`; deployments must provide `BETTER_AUTH_SECRET` explicitly.
- **Board-level approval visibility** — Approvals are now surfaced on issue board views with styled cards, not just in the dedicated approvals page.
