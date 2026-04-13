---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR adds MCP server with approval creation tool, manifest-driven discovery, strict validation, and Docker/CI integration — no tree node covers MCP server architecture or tool surface.
---
# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and IDEs. Source: `/mcp`.

## Purpose

Allows MCP-compatible clients (Claude Desktop, VS Code Copilot, Cursor, etc.) to interact with Paperclip programmatically — creating issues, reading task state, managing approvals — without going through the web UI or REST API directly.

## Key Decisions

### Tool-per-Action Model

Each Paperclip operation exposed via MCP is a discrete tool with its own schema and validation. Request validation is strict — malformed inputs are rejected before reaching business logic. This is a trust boundary: MCP clients are external and untrusted.

### Manifest-Driven Discovery

The MCP server publishes a manifest describing available tools. External agents discover capabilities by reading the manifest rather than probing endpoints. The manifest is included in the Docker build (`deps` stage) and managed through CI so manifest changes can ship independently of lockfile updates.

### Approval Creation Tool

External agents can create approval requests through the MCP `approval-create` tool. This bridges the governance system with external agent toolchains, ensuring agents running outside Paperclip adapters participate in the same oversight model as internal ones.
