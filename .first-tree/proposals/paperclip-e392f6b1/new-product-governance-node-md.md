---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR adds MCP-based approval creation tool — governance node documents approval gates but doesn't mention MCP as an approval creation surface.
---
### MCP Approval Creation

Approvals can be created via the MCP server tool interface, enabling external AI agents and MCP clients to submit approval requests that enter the same governance workflow as approvals created through the REST API or UI. The same server-enforced validation and audit logging applies regardless of creation surface.
