---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: MCP server is a new integration surface for external AI agents with its own trust boundary, tool manifest, and validation layer — no tree node exists for it.
---
# MCP Server

Model Context Protocol server exposing Paperclip capabilities as tools to external AI agents.

**Source:** `packages/mcp/`
**Package:** `@paperclipai/mcp`

---

## Key Decisions

### Tool-Based Interface with Strict Validation

Each MCP tool maps to a discrete Paperclip action declared in a manifest. Request validation at the MCP boundary is independent of and stricter than backend route validation, because external agents operate at a different trust level than authenticated UI users.

**Rationale:** MCP is a trust boundary. External agents don't go through the same auth flow as the UI. Treating every inbound MCP request as untrusted-by-default keeps the attack surface auditable and the tool catalog explicit.

### Shared Services, Separate Package

The MCP server imports backend service functions rather than duplicating business logic. It is a separate package with its own entry point, keeping the protocol boundary clean while reusing the canonical implementation.

**Rationale:** A single source of truth for business logic prevents drift between the UI-facing API and the agent-facing MCP surface.

### Current Tools

- **approval-create** — Lets external agents programmatically request board approval, bridging MCP to Paperclip's governance gates.

### Docker Packaging

The MCP server manifest is included in the Docker deps stage so that layer caching works correctly when only tool definitions change.

---

## Boundaries

- MCP tools are read/write but always subject to governance gates (approvals, budget hard-stops).
- The tool catalog is the API surface — unlisted actions are unreachable.
