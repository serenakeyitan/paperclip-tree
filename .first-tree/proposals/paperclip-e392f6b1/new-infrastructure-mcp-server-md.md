---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: A standalone MCP server package was introduced as a new distributable component with its own Docker manifest inclusion, API validation, and CI handling — this is a new architectural component not covered in the tree.
---
## MCP Server Package

A standalone Model Context Protocol (MCP) server package that exposes Paperclip capabilities to external AI agents and tools.

### Purpose

Allows AI agents running outside of Paperclip's managed adapter system to interact with Paperclip resources (issues, approvals, companies, agents) through the standardized MCP protocol.

### Key Details

- Packaged as a separate distributable (`@paperclipai/mcp-server` or similar).
- Included in the Docker deps stage for containerized deployments.
- API request validation is tightened for security at the MCP boundary.
- CI handles manifest changes independently from lockfile changes.

### Decisions

- Standalone package rather than embedded in the server — allows independent versioning and distribution.
- MCP server validates requests strictly since it's an external trust boundary.
- Manual publish steps are documented separately from the automated release pipeline.
