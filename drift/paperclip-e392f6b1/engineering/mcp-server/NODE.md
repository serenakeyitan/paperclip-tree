---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as MCP tools, allowing external AI agents and clients to interact with Paperclip through the standardized MCP protocol.

## Purpose

The MCP server provides a protocol-native integration surface for AI tools and IDEs that support MCP (e.g., Claude Code, Cursor). Instead of requiring direct REST API calls, external agents can discover and invoke Paperclip operations as MCP tools with structured schemas and validation.

## Key Decisions

### Tool Surface Mirrors Governance Model

MCP tools expose governance-aware operations (e.g., approval creation) that respect the same server-enforced authorization and audit trail as the REST API. The MCP server is a new transport, not a new permission model.

### Manifest-Driven Tool Discovery

The MCP server uses a manifest file to declare available tools. This manifest is included in the Docker build (`deps` stage) and referenced by CI for change detection. Tool additions require manifest updates.

### API Request Validation

MCP tool inputs are validated with tightened schemas before forwarding to the backend API layer, providing an additional validation boundary beyond the REST API's own checks.

## Available Tools

- **Approval creation** — Create approval requests through MCP, enabling external agents to trigger governance gates programmatically.

## Decision Records

_(none yet)_
