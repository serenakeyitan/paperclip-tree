---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend node has no mention of the MCP server, and this PR adds an approval creation tool to it — the MCP server is a distinct interface surface that agents use to interact with Paperclip programmatically.
---
# MCP Server

Model Context Protocol server exposing Paperclip operations as MCP tools for AI agents. Source: `/server/src/mcp/`.

## Purpose

Provides a standardized tool interface so that external AI agents (Claude, Codex, etc.) can interact with Paperclip entities — issues, approvals, agents, projects — without going through the HTTP API directly. The MCP server is included in the Docker image and its manifest is bundled in the deps stage.

## Available Tools

- **Approval creation** — Agents can programmatically create approval requests, enabling governance workflows to be triggered from within agent sessions rather than only through the UI or API.

## Key Decisions

### Validation

API request validation is strict — the MCP layer validates inputs before forwarding to services, preventing malformed requests from reaching business logic. This was tightened after early usage revealed agents sending loosely-typed payloads.
