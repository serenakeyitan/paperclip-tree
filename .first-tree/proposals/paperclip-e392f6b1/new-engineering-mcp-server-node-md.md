---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR adds an MCP approval-create tool, tightens MCP API validation, and includes the MCP manifest in Docker — but the tree has zero nodes covering the MCP server, its tool surface, or its trust boundary role.
---
Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities as MCP tools to external AI agents. Runs as a separate entry point from the Express backend but reuses core service logic.

## Key Decisions

### Separate Package, Shared Services

The MCP server is a distinct package that imports backend services rather than duplicating business logic. This keeps the MCP protocol boundary clean while the backend remains the single source of truth for business rules.

### Trust Boundary Validation

All incoming MCP requests are strictly validated at the boundary before reaching internal services. MCP callers are external agents with different trust characteristics than authenticated UI users, so validation is tightened independently of backend route validation.

### Docker Packaging

The MCP server manifest is included in the Docker deps stage so it is available at build time. Changes to the manifest must be reflected in the Dockerfile.

## Tools

- `approval-create` — Creates approval requests programmatically, enabling agents to trigger governance workflows via MCP.

## Cross-Domain

The MCP server bridges the governance model (approval gates, budget controls) with external agent runtimes. Adapter-connected agents can invoke MCP tools to participate in governance workflows without direct API access.
