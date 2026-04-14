---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR adds approval creation tool (f1bb175) extending governance beyond V1 hiring/strategy gates to programmatic approval requests — existing governance node doesn't cover MCP-initiated approvals.
---
### Programmatic Approval Creation

Approvals can now be created programmatically via the MCP `approval-create` tool, not just through the Board UI or CLI. This extends the governance system's reach to external AI agents connected via MCP. The same server-enforced approval workflow applies — programmatic creation does not bypass any governance checks.
