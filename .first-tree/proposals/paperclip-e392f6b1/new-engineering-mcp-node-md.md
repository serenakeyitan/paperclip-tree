---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR adds MCP approval creation tool (f1bb175), tightens MCP API validation (669e5c8), and includes MCP manifest in Docker (85ca675) — MCP is a significant new surface area with no existing tree node.
---
# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. The MCP server is a protocol adapter — it translates MCP tool calls into Paperclip backend service calls. Business logic remains in backend services, not the MCP layer.

Source: MCP server package within the monorepo.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP protocol. This bridges the governance system (approval gates) with external agent runtimes that connect via MCP rather than the native adapter interface.

### Strict API Request Validation

MCP API requests are validated tightly at the boundary. Invalid or malformed requests are rejected early rather than passed through to backend services. This is a trust boundary — MCP clients may be third-party agents with varying levels of reliability.

### Docker Integration

The MCP server manifest is included in the Docker dependency stage (`deps`) to ensure the MCP server is available in containerized deployments. This follows the same pattern as other workspace packages in the multi-stage Dockerfile.
