---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The existing execution-workspaces node covers worktree isolation, reuse, and scoped checkout, but does not address runtime isolation improvements — ensuring agents cannot interfere with each other's worktree state during concurrent execution.
---
### Runtime Isolation

Beyond filesystem isolation via separate worktrees, the execution workspace system enforces runtime isolation to prevent concurrent agent processes from interfering with each other. This includes ensuring that git operations (fetch, checkout, merge) in one worktree do not lock or conflict with operations in another, and that adapter processes are confined to their assigned worktree's directory tree.

**Rationale:** Filesystem-level worktree separation is necessary but not sufficient. Shared git state (e.g., the main `.git` directory's lock files, index, or ref updates) can create contention when multiple agents execute concurrently. Runtime isolation guards against these shared-state conflicts.
