---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits add MCP server features (approval creation tool, manifest handling, API validation) but no MCP node exists anywhere in the tree.
---
# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request approvals through the governance system.

### Strict API Request Validation

All MCP API requests are tightly validated at the boundary. This prevents malformed or unexpected payloads from reaching internal services.

### Manifest in Docker Deps Stage

The MCP server manifest is included in the Docker `deps` stage so that container builds have access to the tool definitions at build time.

### CI Support for Manifest-Only Changes

CI pipelines support PRs that change only the MCP manifest without requiring lockfile changes, avoiding false failures on manifest-only updates.
