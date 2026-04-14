---
title: "Issue Blockers & Dependency Wakeups"
owners: [cryppadotta]
---

# Issue Blockers & Dependency Wakeups

First-class blocking relationships between issues, with automatic agent wakeups when blockers are resolved.

## Key Decisions

### First-Class Blocker Relationship

Blocking is a distinct, first-class link type — not just a label or generic issue link. When issue A blocks issue B, the system understands the dependency semantics: B cannot proceed until A is resolved. This enables automated workflows that generic links cannot support.

**Rationale:** Agents need machine-readable dependency information to make autonomous scheduling decisions. A generic "related" link doesn't carry enough semantic weight to drive wake/sleep behavior.

### Dependency Wakeups

When a blocking issue is resolved (transitions to `done`), the system automatically wakes agents assigned to the previously-blocked issues. This closes the loop between blocking and the heartbeat/wake system.

**Rationale:** Without automatic wakeups, blocked agents would need to poll or rely on a manager agent to notice unblocks. Automatic wakeups reduce latency and eliminate a coordination burden.

### Relationship to Issue Links

Blocker relationships build on the issue link infrastructure (see `drift/paperclip-e392f6b1/product/task-system/issue-links/NODE.md`) but add semantic behavior (status gating, wake triggers) that plain links don't have.

## Boundaries

- **Wake mechanism** — uses the existing heartbeat wake system documented in `product/task-system/auto-checkout/NODE.md`
- **Link storage** — built on the issue links data model
- **UI rendering** — sub-issues are now displayed inline on the issue detail view rather than in a separate tab
