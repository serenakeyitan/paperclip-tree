---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP server package with an approval creation tool and strict validation patterns — no tree node exists for MCP.
---
# MCP Server

Model Context Protocol server that exposes Paperclip operations as MCP tools, allowing external AI agents to interact with the platform programmatically.

## Key Decisions

### Tool-per-action surface

Each MCP tool maps to a single Paperclip operation (e.g., creating an approval). Tools validate inputs tightly at the boundary before delegating to backend services. This keeps the MCP layer thin — it is a protocol adapter, not a business logic layer.

### Strict API request validation

All incoming MCP requests are validated against strict schemas before reaching service logic. This was tightened to prevent malformed or unexpected payloads from reaching internal services, which is especially important because MCP clients are untrusted external agents.

### Docker packaging

The MCP server manifest is included in the Docker deps stage so that container builds have access to the tool definitions. This ensures the MCP server is deployable alongside the main server without manual manifest copying.
