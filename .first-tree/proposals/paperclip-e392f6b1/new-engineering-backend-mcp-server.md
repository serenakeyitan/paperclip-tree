---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP approval creation tool and MCP server manifest packaging — no tree node exists for the MCP server, its trust model, or its tool catalog.
---
# MCP Server

Standalone MCP (Model Context Protocol) server that exposes Paperclip capabilities as tools for external AI agents. Source: `packages/mcp/`.

## Key Decisions

### Untrusted Boundary

The MCP server runs as a separate process with its own trust boundary. All incoming requests are validated — the server does not trust that MCP clients send well-formed or authorized requests. API request validation is strict.

### Tool Catalog as API Surface

Each MCP tool (e.g., approval creation) is a discrete, documented capability. The tool catalog defines what external agents can do within Paperclip. New tools require explicit addition to the manifest.

### Docker Packaging

The MCP server manifest is included in the Docker deps stage so that containerized deployments have access to the full tool catalog without a separate build step.

### Governance Tools

The MCP server includes tools for governance operations (e.g., creating approvals). This extends governance beyond the board UI into programmatic agent coordination — external agents can request approvals through MCP rather than only through the web interface.
