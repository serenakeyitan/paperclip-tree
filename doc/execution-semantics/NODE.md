Paperclip separates four concepts that are easy to blur together in issue tracking: **structure** (parent/sub-issue relationships), **dependency** (blocker relationships), **ownership** (who is responsible now), and **execution** (whether the control plane has a live path to move the issue forward). Keeping these orthogonal is a load-bearing design decision — the product model, backend orchestration, and UI all depend on it.

## Key Decisions

### Single Assignee Invariant

An issue has at most one assignee. Either `assigneeAgentId` (agent ownership) or `assigneeUserId` (human ownership) may be set, but never both. This is a hard invariant, not a soft convention. Paperclip is single-assignee by design so that ownership questions always have one answer.

### Status Is Not Just a Label

Issue statuses (`backlog`, etc.) imply expectations about ownership and execution — not just UI state. Status transitions drive heartbeat scheduling, wake-on-dependency-resolved behavior, and crash recovery. The canonical status semantics live in `doc/execution-semantics.md`; `doc/SPEC-implementation.md` is the V1 contract and points to execution-semantics for the detailed model.

### Structure vs Dependency vs Ownership vs Execution

Parent/sub-issue is purely structural — it does not imply a blocker relationship or shared ownership. Blockers are an explicit dependency edge (`blockedByIssueIds`) and drive automatic wakeups when resolved. Ownership is the assignee. Execution is whether there is a live run or scheduled heartbeat. A future change that conflates any two of these (for example, auto-blocking on parent status) should be treated as a model change, not a bug fix.

## Source

- `doc/execution-semantics.md` — canonical execution model
- `doc/SPEC-implementation.md` — V1 contract, delegates to execution-semantics for detail
