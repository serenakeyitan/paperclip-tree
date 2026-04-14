---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP approval creation tool (f1bb175), tightens MCP API validation (669e5c8), and fixes MCP server manifest in Docker (85ca675) — but no MCP server node exists in the tree despite proposals already being drafted.
---
# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients.

**Source:** `packages/mcp-server/` (or relevant server MCP routes)

---

## Key Decisions

### Approval Creation Tool

The MCP server exposes a tool for creating approvals programmatically, enabling external agents to trigger governance gates (e.g., hiring approvals, strategy approvals) through the MCP protocol rather than only through the REST API or UI.

### Strict API Request Validation

MCP API requests are tightly validated at the boundary. Input schemas are enforced before any tool execution to prevent malformed or adversarial requests from reaching business logic.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure the MCP server is discoverable and functional in containerized deployments.

---

## Boundaries

- **Governance rules** are enforced by the backend server, not the MCP layer. The MCP server delegates to backend services.
- **Approval semantics** are defined in `product/governance`. This node covers the MCP exposure of those capabilities.
