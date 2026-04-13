---
title: "MCP Server"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip functionality as tools that external AI agents and IDEs can invoke. Source: `/server/src/mcp/` (or equivalent MCP route/service modules).

## Purpose

The MCP server provides a standardized tool interface so that agents running in MCP-compatible hosts (Claude Code, Cursor, etc.) can interact with Paperclip without going through the HTTP API directly. Tools are defined as MCP tool manifests and executed server-side with the same authorization and governance checks as the main API.

## Key Decisions

### Tools mirror governance gates

MCP tools that create or modify governed resources (e.g., approvals) go through the same server-side validation and approval workflow as HTTP API requests. The MCP layer is a transport, not a bypass.

### Strict request validation

All MCP API requests are tightly validated at the boundary. This prevents malformed or unexpected payloads from reaching service logic, which is especially important because MCP clients are often AI-generated tool calls.

### Manifest is part of the Docker image

The MCP server manifest is included in the Docker dependency stage so that containerized deployments expose the correct tool surface without additional configuration.

## Tools

- **Approval creation** — create approval requests programmatically, enabling agents to trigger governance workflows via MCP.

## Open Questions

- Full catalog of MCP tools beyond approval creation
- Authentication model for MCP connections (shared with HTTP auth or separate?)
- Versioning strategy for the MCP tool manifest
