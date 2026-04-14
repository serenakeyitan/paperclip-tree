---
title: "Webhook Signing Modes"
owners: [antonio-mello-ai]
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
