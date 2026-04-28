---
title: "Non-Terminal Issue Liveness Contract"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/NODE.md, product/task-system/issue-blockers/NODE.md]
---

For agent-owned non-terminal issues, Paperclip guarantees that someone or something always owns the next move. An issue is healthy when the product can answer "what moves this forward next?" without a human reconstructing intent from the thread. An issue is stalled when it is non-terminal but has no live execution path, no explicit waiting path, and no recovery path.

## Key Decisions

### Visibility Contract, Not Auto-Completion

The contract is a visibility guarantee, not an auto-completion mechanism. When Paperclip cannot safely infer the next action, it surfaces the ambiguity through a blocked state, a visible comment, or an explicit recovery issue. It must never silently mark work done from prose comments or guess that a dependency is complete.

### Valid Action-Path Primitives

A non-terminal agent-owned issue is healthy when at least one of these holds: an active run linked to the issue; a queued wake or continuation deliverable to the responsible agent; a typed execution-policy participant (e.g. `executionState.currentParticipant`); a pending issue-thread interaction or linked approval awaiting a specific responder; a human owner via `assigneeUserId`; a first-class blocker chain whose unresolved leaf issues are themselves healthy; or an open explicit recovery issue naming owner and action.

### `in_review` Liveness Rule

`in_review` is healthy only when a typed execution participant, pending issue-thread interaction or approval, user owner, active run, queued wake, or explicit recovery issue owns the next action. An `in_review` leaf without any such path is classified as stalled rather than left silently idle.

### Blocked Chain Coverage

A blocked chain is covered only when each unresolved leaf issue is itself live or explicitly waiting. The new `stalled` blocker-attention state (with reason `stalled_review`) propagates up the chain so parent issues surface the stalled leaf rather than appearing healthily blocked.
