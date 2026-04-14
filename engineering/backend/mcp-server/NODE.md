---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip capabilities as tools for external AI agents and IDEs.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP interface. This extends governance capabilities beyond the Paperclip UI and API.

### Strict API Request Validation

MCP API requests are validated strictly — malformed or unexpected fields are rejected rather than silently ignored. This prevents external agents from passing invalid data that could corrupt state.

**Rationale:** MCP clients are untrusted external callers. Strict validation at the MCP boundary protects internal invariants.

Source: PRs `feat(mcp): add approval creation tool`, `fix(mcp): tighten api request validation`, `fix(docker): include mcp server manifest in deps stage`
