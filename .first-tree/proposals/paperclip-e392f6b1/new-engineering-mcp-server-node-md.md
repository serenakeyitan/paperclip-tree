---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits add MCP server features (approval creation tool, validation tightening, Docker manifest inclusion) but no MCP node exists in the tree.
---
# MCP Server

Model Context Protocol server exposing Paperclip functionality to external AI agents. Source: `packages/mcp/`.

Package: `@paperclipai/mcp`

## Key Decisions

### Tool-Based Interface with Strict Validation

The MCP server exposes Paperclip operations as discrete tools (issue management, approval creation, etc.) with strict request validation at the boundary. External agents are untrusted — every input is validated before reaching backend services. This is a deliberate trust boundary.

### Shared Backend Services

MCP tools call the same backend service layer as the HTTP API and frontend. This prevents drift between the UI surface and the MCP surface — a tool and its equivalent UI action always produce identical results.

### Approval Creation Tool

External agents can create approval requests through MCP, enabling governance workflows where an agent proposes an action and a human or governance board approves it. Subject to all standard governance gates (budget limits, board approval).

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that tool discovery works in containerized deployments without requiring a full build.
