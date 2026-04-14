---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title explicitly covers Bearer token redaction from server logs — the security node mentions secret redaction generally but doesn't specifically cover Bearer token log scrubbing.
---
### Bearer Token Redaction in Server Logs

Bearer tokens are explicitly redacted from server log output. This prevents authentication credentials from appearing in log files, log aggregation services, or debug output. The redaction operates at the server logging layer, covering all HTTP request logging.
