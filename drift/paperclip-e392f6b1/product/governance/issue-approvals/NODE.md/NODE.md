---
title: "Issue Approvals"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Approvals

Human-in-the-loop approval gates at the issue level, extending the governance system beyond board-level hiring and strategy approvals.

## Key Decisions

### Multi-Channel Approval Creation

Approvals can be created from three surfaces: the web UI, the CLI (`paperclipai approval` subcommands), and the MCP server (`approval-create` tool). All three converge on the same backend approval service, ensuring consistent validation and audit logging regardless of the creation channel.

### Programmatic Agent Access

Agents can request approvals programmatically via the MCP approval-create tool. This is critical for autonomous workflows where an agent reaches a governance gate and needs human sign-off before proceeding — the agent creates the approval, pauses, and resumes when the approval is resolved.

## Relationship to Governance

This realizes a capability previously listed as deferred in the governance node: "fine-grained per-action governance gates beyond hire and strategy approval." Issue approvals are scoped to individual issues rather than company-wide actions.
