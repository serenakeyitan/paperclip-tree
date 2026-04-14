---
title: "Issue Approvals"
owners: [cryppadotta]
---

# Issue Approvals

Issue-level approval gates extend the governance model beyond the V1 hiring and CEO strategy approvals. Any issue can now have an approval request attached, requiring Board sign-off before the agent proceeds.

## Key Decisions

### Same Approval Infrastructure

Issue approvals reuse the existing approval system (board powers, approval cards, audit trail) rather than introducing a separate mechanism. This keeps governance consistent across all approval types.

### MCP-Accessible

External agents can create approval requests programmatically via the MCP `approval-create` tool, enabling automated governance workflows without direct API access.

### Synchronous Blocking

The approval lifecycle (pending → approved/rejected) follows the same synchronous blocking pattern as hiring approvals: the requesting agent blocks until the Board decides.

### Relationship to Existing Gates

Issue approvals complement but do not replace the V1 hiring and CEO strategy gates. They operate at a finer granularity — per-issue rather than per-category — and can be triggered by agents or humans. This moves "fine-grained per-action governance gates" from deferred to shipped.

Source: PR #3000 (`pap-1167-app-ui-bundle`)
