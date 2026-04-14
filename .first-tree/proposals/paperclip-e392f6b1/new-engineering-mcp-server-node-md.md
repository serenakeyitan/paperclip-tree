---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR adds an MCP approval creation tool and tightens MCP API validation, plus includes MCP server manifest in Docker deps — no MCP node exists anywhere in the tree.
---
# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. Source: MCP-related files in `/server`.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP interface. This extends the governance model's approval gates to agents connecting via MCP, not just the internal adapter system.

### Strict API Request Validation

MCP API requests are validated tightly at the boundary. Invalid or unexpected fields are rejected rather than silently ignored. This prevents external agents from sending malformed requests that could bypass governance checks or produce unexpected state.

### MCP Manifest in Docker Deps Stage

The MCP server manifest file is included in the Docker deps stage alongside workspace package.json files. This ensures the MCP server configuration is available during dependency installation and Docker layer caching works correctly for MCP-related changes.
