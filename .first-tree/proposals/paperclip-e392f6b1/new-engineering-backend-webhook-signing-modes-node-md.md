---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces configurable webhook signing/verification modes (github_hmac, none) — a server security decision with no tree coverage; the backend NODE.md has no mention of webhooks or their verification.
---
# Webhook Signing Modes

The Paperclip server accepts inbound webhooks from external services (GitHub, Sentry, etc.) and supports multiple signing/verification modes to validate webhook authenticity. The signing mode is configurable per integration.

## Supported Modes

- **`github_hmac`** — Validates payloads using GitHub's HMAC-SHA256 signature scheme (`X-Hub-Signature-256` header). Used for GitHub webhook integrations.
- **`none`** — No signature verification. Suitable for development, internal services behind a VPN, or integrations that do not support signing.
- **(default/existing)** — The pre-existing signing mode used before these additions (likely Sentry-style or a generic shared-secret approach).

## Key Decisions

- **Multiple signing modes, not one universal scheme.** Different external services use different signing conventions (GitHub uses HMAC-SHA256, Sentry uses its own format). Rather than forcing all integrations through a single verification path, the server supports mode-specific verification so each integration uses its native signing protocol.
- **`none` mode exists explicitly.** Rather than silently skipping verification when no secret is configured, the `none` mode must be explicitly selected. This makes the security posture of each webhook endpoint visible in configuration.

## Security Implications

Webhook signature verification prevents spoofed payloads from triggering server-side actions. The `none` mode should only be used in trusted network environments. Production deployments receiving webhooks from external services should always use a signing mode that validates authenticity.
