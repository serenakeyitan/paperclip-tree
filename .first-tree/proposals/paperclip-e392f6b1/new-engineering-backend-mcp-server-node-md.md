---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The MCP server (commits f1bb175, 669e5c8, 85ca675) introduces a new subsystem with its own tools, validation, and Docker manifest — no existing node covers MCP.
---
# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools, allowing external AI agents and clients to interact with Paperclip programmatically.

## Key Decisions

### Tool-Based API Surface

The MCP server exposes Paperclip operations as discrete tools (e.g., approval creation, issue management) rather than raw REST proxying. Each tool has typed parameters and validation.

### Strict Request Validation

API requests to the MCP server are validated tightly at the boundary — malformed or unexpected parameters are rejected before reaching business logic. This was tightened after initial deployment revealed overly permissive input handling.

### Server Manifest in Docker Build

The MCP server manifest is included in the Docker dependency stage so that containerized deployments can discover and serve MCP tools without a separate build step.

## Tools

- **Approval creation** — programmatically create approval requests through the governance system
