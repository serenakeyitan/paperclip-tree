---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP tool for creating approvals — a new capability exposing governance actions through the MCP protocol that no existing node documents.
---
# MCP Approval Creation Tool

The MCP server exposes an `approval creation` tool that allows MCP-connected agents to programmatically request approvals through the governance system. This bridges the MCP protocol layer with Paperclip's approval gates.

## Key Decisions

### Approvals via MCP

Agents connected through MCP can now create approval requests without going through the REST API directly. The MCP tool wraps the existing approval service, maintaining the same authorization and validation rules.

**Rationale:** MCP-native agents (e.g., Claude via MCP) need to participate in governance workflows. Exposing approval creation as an MCP tool means these agents can request human sign-off within their natural interaction model rather than making raw HTTP calls.

### Strict Request Validation

The MCP approval tool uses tightened API request validation (also part of this change) to prevent malformed or incomplete approval requests from reaching the governance layer.

## Boundaries

The MCP tool creates approval *requests* only. It does not grant approvals — that remains a human board action per the governance model.
