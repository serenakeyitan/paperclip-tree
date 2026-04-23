---
title: "Issue-Thread Interactions"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/governance/NODE.md", "product/task-system/issue-thread-ux/NODE.md"]
---

# Issue-Thread Interactions

Issue-thread interactions are structured cards rendered inside an issue thread that let an agent solicit explicit input from the board/user through a UI surface instead of free-form markdown. They replace ad-hoc conventions like asking the user to type "yes"/"no" in a comment when the answer controls follow-up work.

## Key Decisions

### Three Card Kinds

Interactions come in three kinds, each with a distinct semantic: `suggest_tasks` (propose child issues for the board/user to accept), `ask_user_questions` (structured questions with typed answers), and `request_confirmation` (explicit accept/reject on a proposal). Yes/no decisions that gate follow-up work must use `request_confirmation` rather than prose, so the decision is machine-readable and auditable.

**Rationale:** Free-form "please reply yes/no" comments are ambiguous to parse and invisible to the rest of the system. Structured cards give the UI, the runtime, and downstream agents a single shape to reason about.

### Separate From Governance Approvals

Interactions are deliberately distinct from the approval system. Approvals are reserved for governed actions that need formal board records (hires, strategy gates, spend, security-sensitive actions). Ordinary issue-scoped decisions — plan acceptance, proposal confirmation, breakdown review — use interactions instead, keeping the approval audit trail focused on governance.

**Rationale:** Collapsing the two surfaces would either dilute the governance audit trail with routine decisions or force governed actions to travel through an issue-scoped channel. Keeping them distinct preserves the signal of both.

### Idempotency and Supersession

Interactions carry an `idempotencyKey` scoped by `(company_id, issue_id, idempotency_key)` via a partial unique index, so retrying a create with the same key is safe. The canonical plan-approval key is `confirmation:{issueId}:plan:{revisionId}`, binding the card to an exact plan revision. When `supersedeOnUserComment: true`, a later board/user comment invalidates the pending card; if the decision is still needed, the agent revises the proposal and creates a fresh confirmation.

**Rationale:** Agents retry. Without idempotency the thread fills with duplicate cards. Without supersession a stale card remains actionable after the human has already moved the conversation on, which creates decision drift.

### Continuation Policy

`continuationPolicy: "wake_assignee"` tells the heartbeat runtime to wake the assignee when the interaction resolves. For `request_confirmation` specifically, only acceptance wakes the assignee by default — rejection leaves continuation to a normal board/user comment with revision notes.

**Rationale:** Rejection usually means "change the plan," which is a substantive reply. Auto-waking on rejection invites the agent to loop before the human has finished articulating what to change.

### Plan Approval Flow

Plan approval is the canonical use case: update the `plan` issue document, read back the saved `documentId`/`latestRevisionId`, create a `request_confirmation` targeting that revision, and wait for acceptance before creating implementation subtasks. This replaces the older pattern of asking for plan approval in markdown.

**Rationale:** Binding the confirmation to an exact revision makes "approved plan" a concrete artifact, not a moving target. Implementation subtasks created after acceptance can cite the revision they were authorized against.

## Relationship to Other Nodes

- **Issue Thread UX** ([`../issue-thread-ux/NODE.md`](../issue-thread-ux/NODE.md)) — interactions are rendered *inside* the thread but are a distinct surface from polished markdown comments; thread UX decisions about rendering comments do not govern card rendering.
- **Governance approvals** (`product/governance/`) — the deliberate boundary between routine issue-scoped decisions (here) and governed board-level actions (there).
