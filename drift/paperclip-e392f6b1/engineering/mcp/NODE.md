---
title: "MCP Server"
owners: []
---

## Summary

Paperclip exposes an MCP (Model Context Protocol) server that lets external AI agents interact with the platform through standardized tool calls. The MCP server provides tools for creating approvals, managing issues, and other governance-related actions.

## Key Decisions

- **Tool-based interface.** Each capability (e.g., approval creation) is exposed as a discrete MCP tool with a typed manifest. The manifest is included in the Docker deps stage so it is available at runtime.
- **Strict request validation.** All incoming MCP API requests are validated tightly to prevent malformed or unauthorized tool invocations. Validation happens at the MCP layer before reaching backend services.
- **Governance integration.** MCP tools respect the same approval gates and budget controls as the rest of the platform — creating an approval via MCP follows the same governance rules as the UI.

## Relationship to Other Domains

- **Backend** — MCP routes are served by the backend Express server.
- **Governance** — MCP tools that create approvals or mutate governed resources go through the same approval and audit pipeline.
- **Infrastructure/Deployment** — The MCP server manifest must be bundled in the Docker image.
