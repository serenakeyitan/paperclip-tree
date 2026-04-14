---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR adds an MCP approval creation tool, tightens MCP API request validation, and includes the MCP server manifest in Docker — no existing node covers the MCP server layer.
---
# MCP Server

The MCP (Model Context Protocol) server exposes Paperclip capabilities as tools that external AI agents can invoke via the standardized MCP protocol. This provides a tool-based interface for agents to interact with Paperclip's task system, governance, and other domains without direct REST API access.

## Key Decisions

### Approval Creation Tool

The MCP server includes a tool for creating approval requests, bridging the governance approval gate system to external agents via MCP. This allows MCP-connected agents to request human approvals through the standard tool interface rather than calling the REST API directly.

### Tight Boundary Validation

MCP API requests are validated strictly at the boundary layer to prevent malformed or unauthorized tool invocations from reaching backend services. This is the MCP equivalent of route-level input validation in the REST API.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that the MCP tool surface is available in containerized deployments without requiring a separate build step.
