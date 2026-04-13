---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: A standalone MCP server package was added with Docker integration, approval creation tools, and validation hardening — a new architectural component with no tree node.
---
## Overview

Paperclip includes a standalone MCP (Model Context Protocol) server package that exposes Paperclip functionality as MCP tools. This allows external AI agents and clients to interact with Paperclip through the MCP standard.

## Key Decisions

- **Standalone package** — The MCP server is a separate package within the monorepo, not embedded in the main server. This keeps deployment flexible and allows independent versioning.
- **Docker inclusion** — The MCP server manifest is included in the Docker deps stage, ensuring it's available in production containers.
- **Approval creation tool** — The MCP server exposes an approval creation tool, enabling external agents to trigger approval workflows.
- **Request validation** — API request validation is tightened to prevent malformed or malicious input from MCP clients.
- **Manual publish workflow** — MCP server releases follow a manual publish process documented separately from the main CI/CD pipeline.
