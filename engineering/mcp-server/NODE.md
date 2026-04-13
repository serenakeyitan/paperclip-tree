---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and IDEs.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents connected via MCP to programmatically create approval requests within Paperclip's governance system. This bridges the governance layer (approval gates, board review) with external tooling.

### Strict API Request Validation

All MCP tool invocations pass through tightened request validation before reaching backend services. This prevents malformed or unexpected payloads from external MCP clients from reaching the core API.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure the MCP server is available in containerized deployments. This was a deployment concern — the manifest was initially missing from the deps layer.

### CI Pipeline for MCP Changes

Manifest-only changes to the MCP server are handled without requiring lockfile updates in CI, keeping the PR pipeline clean when only the MCP tool surface changes.
