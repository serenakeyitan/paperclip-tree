---
title: "MCP Server"
owners: []
---

## Overview

Paperclip exposes an MCP (Model Context Protocol) server that lets external AI agents interact with Paperclip resources through standardized tools. The MCP server is packaged alongside the main application and its manifest is included in the Docker deps stage.

## Key Decisions

- **Tool-based interface.** Each MCP tool maps to a specific Paperclip action (e.g. creating approvals, querying issues). Tools are declared in a manifest and validated at the API boundary.
- **Strict request validation.** All incoming MCP requests are validated before reaching business logic to prevent malformed or unauthorized operations.
- **Approval creation tool.** Agents can programmatically request board approvals via the MCP `approval-create` tool, connecting MCP to the governance approval gate system.

## Relationships

- Governance approval gates are defined in `product/governance/`.
- Docker packaging is covered in `infrastructure/deployment/`.
