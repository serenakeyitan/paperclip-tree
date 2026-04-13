---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP approval creation tool and tightens MCP request validation, but the tree has no node documenting the MCP server, its tool-per-action model, or manifest-driven discovery.
---
# MCP Server

Model Context Protocol server exposing Paperclip actions as discrete tools for MCP-connected agents. Source: `/mcp-server`.

## Architecture

### Tool-per-Action Model

Each Paperclip capability (issue CRUD, approval creation, project management) is exposed as a separate MCP tool. Tools use strict request validation to reject malformed inputs before they reach service logic.

### Manifest-Driven Discovery

The MCP server publishes a tool manifest that MCP clients use to discover available capabilities. The manifest is included in the Docker image at the `deps` stage to ensure it ships with every deployment.

## Key Decisions

### Approval Creation via MCP

MCP-connected agents can create approval requests through the governance system without raw HTTP calls. The MCP tool wraps the existing approval service, maintaining the same authorization and validation rules. It creates approval *requests* only — granting approvals remains a human board action.

**Rationale:** MCP-native agents (e.g., Claude) need to participate in governance workflows within their natural interaction model.

### Strict Request Validation

All MCP tool inputs are validated before reaching business logic, preventing malformed requests from bypassing the protocol boundary.
