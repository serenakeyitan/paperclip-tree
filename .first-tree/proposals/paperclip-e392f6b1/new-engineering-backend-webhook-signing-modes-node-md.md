---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend NODE.md has no mention of webhooks or webhook signing/verification; this PR introduces a new configurable signing mode system (github_hmac, none) for inbound webhooks that represents a distinct server security decision.
---
# Webhook Signing Modes

The Paperclip server accepts inbound webhooks from external services (GitHub, Sentry, etc.) and supports multiple signing/verification modes to validate webhook authenticity. The signing mode is configurable per integration.

## Supported Modes

- **`github_hmac`** — Validates payloads using GitHub's HMAC-SHA256 signature scheme (`X-Hub-Signature-256` header). Used for GitHub webhook integrations.
- **`none`** — Skips signature verification entirely. For trusted-network or development environments only.

## Key Decisions

- **`none` mode must be explicitly selected.** Rather than silently skipping verification when no secret is configured, the `none` mode must be explicitly chosen. This makes the security posture of each webhook endpoint visible in configuration.
- **Mode is per-integration, not global.** Each webhook source can have its own signing mode, allowing mixed security levels across integrations.

## Security Notes

Webhook signature verification prevents spoofed payloads from triggering server-side actions. The `none` mode should only be used in trusted network environments. Production deployments receiving webhooks from external services should always use a signing mode that validates authenticity.
