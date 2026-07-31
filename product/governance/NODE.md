---
title: "Governance"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/agent-model/NODE.md", "product/company-model/NODE.md", "product/task-system/NODE.md", "engineering/NODE.md"]
---

# Governance

Board powers, approval gates, budget controls, and cost tracking -- the human oversight layer.

## Key Decisions

### Single Human Board (V1)

Every Company has a Board that governs high-impact decisions. V1: one human operator per deployment. The Board is not just an approval gate -- it is a live control surface. The human can intervene at any level at any time.

**Rationale:** Conservative default. Human control is non-negotiable for V1. Multi-member boards, delegated authority, and hiring budgets are deferred to avoid premature complexity in the governance model.

### Board Powers (Always Available)

The Board has **unrestricted access** to the entire system:

- Set and modify Company budgets at any level
- Pause/resume any Agent immediately
- Pause/resume any work item (paused items are not picked up by agents)
- Full project management access (create, edit, comment on, reassign, delete any task/project/milestone)
- Override any Agent decision (reassign tasks, change priorities, modify descriptions)
- Approve/reject/cancel pending approvals
- Terminate agents (irreversible)

**Rationale:** The board must always be able to stop anything, change anything, or override anything. This is the safety foundation that makes autonomous operation acceptable.

### Approval Gates

Three gates are implemented:

1. **Hiring** -- New agent creation requires Board approval (agents propose hires via the approval system; Board can also create agents directly, which is still logged as a governance action)
2. **CEO Strategy Approval** -- CEO's initial strategic breakdown (org structure, sub-tasks, hiring plan) must be approved before execution begins. Before first strategy approval, CEO may only draft tasks, not transition them to active execution states.
3. **Issue Approvals** -- Individual issues can require human sign-off before work proceeds. External agents can create approval requests via the MCP `approval-create` tool. See [issue-approvals/NODE.md](issue-approvals/NODE.md).

**Rationale:** Hiring and strategy approval are the two highest-leverage fixed checkpoints. Issue approvals extend governance to individual work items at finer granularity, allowing agents and humans to gate specific tasks without requiring a new category-level gate.

### Budget Hard-Stops

Token/LLM cost budgeting is core (not a plugin). Three tiers:

1. **Visibility** -- dashboards showing spend at every level (Agent, task, project, Company)
2. **Soft alerts** -- configurable thresholds (default: 80% of budget)
3. **Hard ceiling** -- at 100%, auto-pause the Agent, block new checkout/invocation, emit high-priority activity event. Board notified. Board can override by raising budget or explicitly resuming.

Budgets can be set to **unlimited** (no ceiling). Budget period is **monthly UTC calendar window** for V1.

**Rationale:** "Safe autonomy" is a core design goal. Auto mode is allowed; hidden token burn is not. The hard ceiling is the critical safety mechanism -- it guarantees that no agent can silently drain resources.

### Cascading Budget Delegation

The Board sets Company-level budgets. The CEO can set budgets for agents below them, and every manager agent can set budgets for their reports. The Board can manually override any budget at any level.

**Rationale:** This mirrors real corporate budget delegation. The CEO gets a budget from the board and distributes it. Managers distribute their allocation. The board retains override power.

### Cost Tracking at Every Level

Fully-instrumented agents report token/API usage. Costs are tracked per Agent, per task, per project, and per Company, denominated in both tokens and dollars. Billing codes on tasks enable cross-team cost attribution.

Cost rollups are read-time aggregate queries for V1. Materialized rollups can be added later if query latency becomes a problem.

### Governance Enforced at Server Layer

Governance rules are control-plane invariants enforced at the server, not at the adapter or agent level. Agents cannot bypass governance checks. The approval workflow is synchronous: agents block until approval is granted or denied.

### Unified API, Different Authorization

The same REST API serves both the Board UI and agents. Authentication determines permissions: board auth has full access, agent API keys have scoped access (own tasks, cost reporting, company context). No separate "agent API" vs. "board API."

**Rationale:** One API surface is simpler to maintain and reason about. Authorization-based scoping is more flexible than separate endpoints.

### Auditable Everything

Every mutation writes to `activity_log`. Every approval decision is logged. Agent API keys are hashed (plaintext shown once at creation). Secrets are redacted in logs and API responses. The system is designed so that nothing happens silently.

**Rationale:** "Surface problems, don't hide them" is a core principle. The audit trail is what makes autonomous operation trustworthy.

## Deferred Governance Features

- Multi-member boards
- Hiring budgets (auto-approve hires within $X/month)
- Delegated authority (CEO can hire within limits)
- Role-based human permission granularity

## Decision Records

- [member-archival.md](member-archival.md) — Why human company members are archived through a dedicated governance flow instead of being deleted or generically status-edited.
- [server-enforced-approvals-and-budget-stops.md](server-enforced-approvals-and-budget-stops.md) — Why approvals, budget hard stops, and their audit trail are enforced in the server rather than delegated to agents or adapters.

## Sub-domains

- **[issue-approvals/](issue-approvals/NODE.md)** -- Per-issue approval gates, MCP-accessible approval creation, and integration with the board approval workflow.
- `multi-user-access/` — Multi-User Access & Invites

## Open Questions

- Budget period flexibility (per-day, per-week, rolling) beyond the V1 monthly window
- Dashboard design -- which metrics matter most at each governance level
