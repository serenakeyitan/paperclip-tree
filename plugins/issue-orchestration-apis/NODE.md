---
title: "Plugin Issue Orchestration APIs"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md, product/governance/NODE.md, product/task-system/NODE.md, product/task-system/issue-blockers/NODE.md]
---

Trusted orchestration plugins create and manage Paperclip issues through `ctx.issues` rather than importing server internals. The public contract covers parent/project/goal links, board or agent assignees, blocker IDs, labels, billing code, request depth, execution-workspace inheritance, and plugin origin metadata.

**Origin governance.** Built-in issues keep core origins like `manual` and `routine_execution`. Plugin-managed issues use `plugin:<pluginKey>` (or sub-kinds such as `plugin:<pluginKey>:feature`). The host derives the default origin from the installed plugin key and rejects attempts to write `plugin:<otherPluginKey>` origins. `originId` is plugin-defined and should be stable so generated work is idempotent.

**Relations and reads.** `ctx.issues.relations` exposes `get`, `setBlockedBy`, `addBlockers`, and `removeBlockers`. `ctx.issues.getSubtree` and `ctx.issues.summaries.getOrchestration` give plugins structured views of issue trees and rolled-up orchestration state without bespoke SQL.

**Governance helpers.** `ctx.issues.assertCheckoutOwner` lets a plugin action preserve agent-run checkout ownership before mutating an issue. `ctx.issues.requestWakeup` (and the batch `requestWakeups`) routes wakeups through host heartbeat semantics — terminal-status, blocker, assignee, and budget hard-stop checks all run host-side. Batch wakeups accept an idempotency-key prefix so coordinator retries stay stable.

This surface is capability-gated (e.g. `issues.create`, `issues.wakeup`, `issue.relations.write`, `issue.subtree.read`, `issues.orchestration.read`) and is the supported way for orchestration plugins to drive multi-issue workflows.
