---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as tools for external AI agents. Source: `/packages/mcp-server`.

## Key Decisions

### Standalone Package

The MCP server is a separate package (`@paperclipai/mcp-server`) rather than embedded in the main server, allowing independent versioning and keeping the MCP protocol boundary clean.

### Approval Creation Tool

The MCP server exposes an approval creation tool, enabling external agents to programmatically request governance approvals through the MCP protocol.

### Strict API Request Validation

All incoming MCP tool inputs are validated strictly before forwarding to internal services. MCP clients are external and untrusted — this is a critical trust boundary.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure availability in containerized deployments.

## Boundaries

- **Governance integration** — approval tool delegates to the same approval service used by the main server
- **Backend server** — MCP server calls internal service APIs, does not duplicate business logic
