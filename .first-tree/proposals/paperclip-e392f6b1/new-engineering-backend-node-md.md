---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend node documents routes, auth, and config but has zero mention of webhooks or webhook signing — this PR adds a new signing mode subsystem that future agents need to know about.
---
### Webhooks

The server supports inbound webhooks (e.g., from GitHub, Sentry) with configurable signature verification. Signing modes:
- **`github_hmac`** — HMAC-SHA256 verification using GitHub's `X-Hub-Signature-256` header format.
- **`none`** — No signature verification (for trusted-network or development use only).

Signing mode is configured per webhook source. This allows Paperclip to accept events from multiple external systems, each with its own authentication scheme.
