---
title: "Webhook Signing Modes"
owners: [bingran-you, cryppadotta, serenakeyitan, antonio-mello-ai]
---

# Webhook Signing Modes

The Paperclip server accepts inbound webhooks from external services (GitHub, Sentry, etc.) and supports multiple signing/verification modes to validate webhook authenticity. The signing mode is configurable per integration.

## Supported Modes

- **`bearer`** — Validates webhooks using a Bearer token in the `Authorization` header. Pre-existing mode.
- **`hmac_sha256`** — Generic HMAC-SHA256 signature verification using a shared secret. Pre-existing mode.
- **`github_hmac`** — Validates payloads using GitHub's HMAC-SHA256 signature scheme (`X-Hub-Signature-256` header). Added in PR #1961 for GitHub webhook integrations.
- **`none`** — No signature verification. Suitable for development, internal services behind a VPN, or integrations that do not support signing. Added in PR #1961.

## Key Decisions

- **Multiple signing modes, not one universal scheme.** Different external services use different signing conventions (GitHub uses HMAC-SHA256, Sentry uses its own format). Rather than forcing all integrations through a single verification path, the server supports mode-specific verification so each integration uses its native signing protocol.
- **`none` mode exists explicitly.** Rather than silently skipping verification when no secret is configured, the `none` mode must be explicitly selected. This makes the security posture of each webhook endpoint visible in configuration.

## Security Implications

Webhook signature verification prevents spoofed payloads from triggering server-side actions. The `none` mode should only be used in trusted network environments. Production deployments receiving webhooks from external services should always use a signing mode that validates authenticity.
