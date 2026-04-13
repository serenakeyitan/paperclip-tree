---
title: "Issue-Level Approvals"
owners: []
---

Issue-level approval gates extend the governance model beyond the V1 hiring and CEO strategy approvals. Any issue can now have an approval request attached, requiring Board sign-off before the agent proceeds.

## How It Works

Approval requests can be created on issues through the Board UI or programmatically via the MCP `approval-create` tool. This means external agents connected through adapters can request approval before proceeding with high-stakes actions — closing the loop between autonomous operation and human oversight.

Approval cards are rendered on the board with dedicated styling. The approval lifecycle (pending → approved/rejected) follows the same synchronous blocking pattern as hiring approvals: the requesting agent blocks until the Board decides.

## Relationship to Governance Model

This is the first implementation of the "fine-grained per-action governance gates" listed as deferred in the governance node. It moves issue-level approval from deferred to shipped.

## Technical Surface

The database schema includes `issue_approvals` alongside the existing `approvals` table. The MCP server (`engineering/mcp-server`) exposes the `approval-create` tool. The frontend renders approval cards inline on the issue board.
