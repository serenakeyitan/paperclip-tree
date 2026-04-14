---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The existing freshness node covers document revision tracking but not broader live update patterns for issue workflow state introduced in this PR.
---
### Live Updates Beyond Document Revisions

Live update patterns extend beyond document content freshness to cover issue workflow state — status, assignment, and other mutable fields are kept current through the same frontend-driven reconciliation approach. This ensures the entire issue view (not just the document pane) reflects the latest server state.

Source: PR #3222 (`pap-1266-issue-workflow`)
