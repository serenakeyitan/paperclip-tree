---
title: "MCP Server"
owners: []
---

## Overview

Standalone Model Context Protocol (MCP) server package that exposes Paperclip capabilities to MCP-compatible AI clients (Claude Desktop, IDE extensions, and other MCP hosts). The server is a separate package from the Express backend but reuses the same core service logic.

## Key Decisions

- **Standalone package, not embedded in the backend.** The MCP server runs as its own process with a typed tool manifest, keeping the MCP transport concern out of the HTTP server.
- **Reuses core service logic.** MCP tool handlers delegate to the same services that back the REST API — no duplicated business logic.
- **Company-scoped access.** MCP connections are scoped to a single company, maintaining multi-tenant isolation.
- **Strict request validation.** Input is validated at the MCP layer before reaching backend services, tightening the trust boundary for external agent callers.
- **Docker-aware.** The MCP server manifest is included in the Docker deps stage so the image can serve MCP without a rebuild.

## Current Tools

- **Approval creation** — External agents can create approval requests through the MCP interface, respecting the same governance gates as the UI.

## Relationship to Other Domains

The MCP server sits at the intersection of the backend (shares services), governance (approval tool respects approval gates), and adapters (MCP-compatible agents can call Paperclip tools). As more tools are added, each should mirror an existing backend capability rather than introduce new business logic.
