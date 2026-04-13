---
title: "Webhook Signing Modes"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Webhook Signing Modes

Paperclip supports multiple webhook signing modes for outbound webhook delivery.

## Supported Modes

- **`github_hmac`** — HMAC-SHA256 signature via the `X-Hub-Signature-256` header. Compatible with GitHub's webhook verification pattern.
- **`none`** — No signature. Must be explicitly selected; this is never a silent default when no secret is configured.

## Key Decisions

### Explicit Opt-In for Unsigned

The `none` mode requires explicit selection. If no signing mode is configured and no secret is provided, webhook delivery fails rather than silently sending unsigned payloads. This prevents accidental exposure of webhook data without integrity verification.
