---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The MCP server is an entirely new integration surface (packages/mcp/) with its own trust model, tool catalog, and Docker packaging — no tree node exists for it.
---
# MCP Server

Standalone Model Context Protocol server exposing Paperclip capabilities as tools to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Source: `packages/mcp/`.

Package: `@paperclipai/mcp`

## Key Decisions

### Separate Process, Shared Services

The MCP server runs as its own process but imports backend service modules directly rather than calling the REST API. This avoids duplicating business logic while keeping the MCP process independently deployable.

### Untrusted Trust Boundary

MCP is treated as an untrusted boundary — input validation at the MCP layer is stricter than internal backend route validation because MCP clients are not authenticated the same way as UI users. Every tool call is scoped to a `company_id` (platform-wide isolation applies).

### Tool Catalog as API Surface

The MCP manifest defines the complete set of available tools. Unlisted actions are unreachable. The manifest is included in the Docker deps stage for correct layer caching.

### First Tool: Approval Creation

The first shipped MCP tool is `approval-create`, allowing external agents to programmatically request approval on any issue. This bridges MCP into Paperclip's governance layer — agents hitting a governance gate can request sign-off without human intervention in the UI.
