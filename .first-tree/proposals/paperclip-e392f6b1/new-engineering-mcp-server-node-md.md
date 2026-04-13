---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds an MCP approval creation tool, tightens API validation, and references a manifest-based discovery system — no tree node documents the MCP server architecture, tool registry, or manifest system.
---
# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as tools for external AI agents. Source: `/mcp`.

## Key Decisions

### Manifest-Based Tool Discovery

Available tools are declared in a manifest file that ships with the Docker image (included in the deps stage for layer caching). External agents discover capabilities by reading the manifest rather than probing endpoints.

### Strict API Request Validation

All incoming MCP requests are validated against tight schemas before reaching service logic. This is a trust boundary — MCP clients are external and untrusted. Validation rejects malformed requests early rather than relying on downstream service checks.

### Approval Creation Tool

External agents can create approval requests through the MCP `approval-create` tool. This bridges the governance system with external agent toolchains, allowing agents running outside Paperclip adapters to request human approval for governed actions.

## Boundaries

The MCP server is the primary integration surface for agents that don't use a native Paperclip adapter. Governance primitives exposed here ensure external agents participate in the same oversight model as internal ones.
