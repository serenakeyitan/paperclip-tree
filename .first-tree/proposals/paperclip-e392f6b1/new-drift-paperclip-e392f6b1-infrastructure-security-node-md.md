---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR redacts Bearer tokens from server log output — a new security hardening decision not covered by the existing security node which only mentions secret handling at rest and in API responses.
---
### Bearer token redaction in logs

Server log output must redact Bearer tokens from HTTP headers before writing to any log sink. This prevents authentication credentials from appearing in log files, log aggregation services, or debug output. Combined with the existing convention that secrets are redacted in API responses, this closes the log-side exposure vector.

**Rationale:** Bearer tokens in logs are a common credential leak path — log files are often stored with weaker access controls than secret stores, and may be shipped to third-party log aggregation services.
