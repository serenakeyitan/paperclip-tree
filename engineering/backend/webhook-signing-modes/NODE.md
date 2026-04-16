---
title: "Webhook Signing Modes"
owners: [bingran-you, cryppadotta, serenakeyitan, antonio-mello-ai]
---

# Webhook Signing Modes

Signing/verification modes for routine webhook triggers. When a routine trigger is configured with `kind: "webhook"`, the server validates inbound payloads using one of the supported signing modes before firing the routine. The signing mode is set per trigger.

## Supported Modes

- **`bearer`** — Validates webhooks using a Bearer token in the `Authorization` header. Pre-existing mode.
- **`hmac_sha256`** — Generic HMAC-SHA256 signature verification using a shared secret. Pre-existing mode.
- **`github_hmac`** — Validates payloads using HMAC-SHA256 via the `X-Hub-Signature-256` header (or falls back to `X-Paperclip-Signature`). Compatible with GitHub webhooks, Sentry, and any service following the same standard. No timestamp-based replay protection. Added in PR #1961.
- **`none`** — No authentication; the 24-character hex `publicId` in the trigger URL acts as the shared secret. Suitable for development, internal services behind a VPN, or services that cannot add custom auth headers. Added in PR #1961.

## Key Decisions

- **Multiple signing modes, not one universal scheme.** Different external services use different signing conventions. GitHub and Sentry both use HMAC-SHA256 via `X-Hub-Signature-256`, so `github_hmac` covers both. The `none` mode exists for services that cannot add custom auth headers (e.g., some Sentry webhook configurations). Rather than forcing all integrations through a single verification path, the server supports mode-specific verification so each routine trigger can match its source's native signing protocol.
- **`none` mode exists explicitly.** Rather than silently skipping verification when no secret is configured, the `none` mode must be explicitly selected. This makes the security posture of each webhook endpoint visible in configuration.

## Security Implications

Webhook signature verification prevents spoofed payloads from triggering server-side actions. The `none` mode should only be used in trusted network environments. Production deployments receiving webhooks from external services should always use a signing mode that validates authenticity.
