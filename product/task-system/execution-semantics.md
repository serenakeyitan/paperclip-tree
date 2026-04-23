---
title: "Execution Semantics"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links:
  - "product/task-system/NODE.md"
  - "product/task-system/issue-blockers/NODE.md"
  - "product/agent-model/NODE.md"
---

# Execution Semantics

Paperclip separates four concepts that are easy to blur together in issue tracking: **structure** (parent/sub-issue relationships), **dependency** (blocker relationships), **ownership** (who is responsible now), and **execution** (whether the control plane has a live path to move the issue forward). Keeping these orthogonal is a load-bearing design decision — the product model, backend orchestration, and UI all depend on it.

## Key Decisions

### Single Assignee Invariant

An issue has at most one assignee. Either `assigneeAgentId` (agent ownership) or `assigneeUserId` (human ownership) may be set, but never both. This is a hard invariant, not a soft convention. Paperclip is single-assignee by design so that ownership questions always have one answer.

**Rationale:** Diffuse ownership is the single biggest failure mode for autonomous agents — if two assignees can exist, neither acts, or both act and conflict. Enforcing the invariant at the model level (rather than by convention) means every downstream system — checkout, wake routing, dashboards — can trust it without re-checking.

### Status Is Not Just a Label

Issue statuses (`backlog`, `todo`, `in_progress`, `in_review`, `done`, `blocked`, `cancelled`) imply expectations about ownership and execution — not just UI state. Status transitions drive heartbeat scheduling, wake-on-dependency-resolved behavior, and crash recovery. The canonical status semantics live in `doc/execution-semantics.md`; `doc/SPEC-implementation.md` is the V1 contract and points to execution-semantics for the detailed model.

**Rationale:** If status were cosmetic, agents could not use it to coordinate. Making status load-bearing means the orchestrator can safely schedule, wake, and recover work based purely on status transitions, without a parallel execution-state field.

### Structure vs Dependency vs Ownership vs Execution

Parent/sub-issue is purely **structural** — it does not imply a blocker relationship or shared ownership. Blockers are an explicit **dependency** edge (`blockedByIssueIds`) and drive automatic wakeups when resolved. **Ownership** is the assignee. **Execution** is whether there is a live run or scheduled heartbeat.

A future change that conflates any two of these (for example, auto-blocking on parent status, or treating "has a run attached" as a dependency edge) should be treated as a model change, not a bug fix — it requires an explicit tree update here.

**Rationale:** Conflating these concepts is the easy, tempting mistake. Each conflation looks like a small convenience in isolation (auto-propagating status from parent to sub-issue, auto-blocking when a sub-issue is in progress, etc.) but each one breaks a different downstream invariant — dependency wakeups, checkout, or crash recovery. Declaring them orthogonal up front is what lets every downstream system reason locally.

## Source

- `doc/execution-semantics.md` — canonical execution model (paperclipai/paperclip)
- `doc/SPEC-implementation.md` — V1 contract, delegates to execution-semantics for detail

## Relationship to Other Nodes

- **[issue-blockers/](issue-blockers/NODE.md)** — details the dependency edge and wake behavior summarized here.
- **[../agent-model/NODE.md](../agent-model/NODE.md)** — defines assignee types (agent vs human) that the single-assignee invariant ranges over.
