---
supersedes: product/governance/NODE.md
source_commit: b649bd454fce0c5d9aed64e6b75eb302b5d255ba
---

## Issue-Linked Approvals

The approval system has expanded beyond hiring and strategy gates to support generic issue-linked approvals visible on board views.

### Changes from Original Model

- **Generic issue-linked approvals** — Any issue can now have associated approvals, not just hiring and CEO strategy decisions. Approvals appear as cards on board views.
- **Board approval visibility** — Approval cards are styled and visible on kanban boards, making pending approvals discoverable in the normal workflow.
- **MCP approval tool** — An MCP server tool (`approval creation`) allows external systems and agents to create approvals programmatically.
- **Authorization scoping** — Approval routes are now properly scoped alongside import, activity, and heartbeat routes for security.

### Decisions

- Approvals are generic rather than type-specific, allowing the same approval UX for any workflow that needs human sign-off.
- MCP integration enables approval creation from agent toolchains, not just the web UI or CLI.
