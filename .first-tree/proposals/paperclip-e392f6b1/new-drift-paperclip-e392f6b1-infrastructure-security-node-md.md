---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The security node covers secret handling and auth but not webhook signing verification — the new signing modes have security implications (especially the 'none' mode) that should be documented.
---
### Webhook signing verification

Inbound webhooks support multiple signing modes. The `github_hmac` mode verifies payloads using HMAC-SHA256 with a shared secret. The `none` mode disables verification entirely and should only be used in trusted-network or local development contexts. Each webhook source can be configured independently, so a Sentry webhook and a GitHub webhook can use different signing strategies.
