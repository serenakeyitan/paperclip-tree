---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend NODE.md has no mention of webhooks or their signing/verification; this PR introduces a multi-mode webhook signing system (github_hmac, none) with explicit opt-in for unsigned payloads — a security-relevant architectural decision.
---
# Webhook Signing Modes

The Paperclip server accepts inbound webhooks from external services (GitHub, Sentry, etc.) and supports multiple signing/verification modes to validate webhook authenticity. The signing mode is configurable per integration.

## Supported Modes

- **`github_hmac`** — Validates payloads using GitHub's HMAC-SHA256 signature scheme (`X-Hub-Signature-256` header). Used for GitHub webhook integrations.
- **`none`** — No signature verification. Suitable for development, internal services behind a VPN, or integrations that do not support signing.
- **(default)** — The pre-existing signing mode used before these additions.

## Key Decisions

### Multiple signing modes, not one universal scheme

Different external services use different signing conventions (GitHub uses HMAC-SHA256, Sentry uses its own format). Rather than forcing all integrations through a single verification path, the server supports mode-specific verification so each integration uses its native signing protocol.

### Explicit opt-in for unsigned webhooks

The `none` mode must be explicitly selected. If no signing mode is configured and no secret is provided, webhook processing fails rather than silently accepting unverified payloads. This makes the security posture of each webhook endpoint visible in configuration and prevents accidental exposure.

## Security Implications

Webhook signature verification prevents spoofed payloads from triggering server-side actions. The `none` mode should only be used in trusted network environments. Production deployments receiving webhooks from external services should always use a signing mode that validates authenticity.
