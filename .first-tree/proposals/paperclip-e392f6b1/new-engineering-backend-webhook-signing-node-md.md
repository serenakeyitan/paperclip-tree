---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Webhook signing modes (github_hmac vs none) with explicit opt-in for unsigned webhooks is a new security convention not in the tree.
---
# Webhook Signing Modes

Paperclip supports multiple webhook signing modes for outbound webhook delivery.

## Supported Modes

- **`github_hmac`** — HMAC-SHA256 signature via the `X-Hub-Signature-256` header. Compatible with GitHub's webhook verification pattern.
- **`none`** — No signature. Must be explicitly selected; this is never a silent default when no secret is configured.

## Key Decisions

### Explicit Opt-In for Unsigned

The `none` mode requires explicit selection. If no signing mode is configured and no secret is provided, webhook delivery fails rather than silently sending unsigned payloads. This prevents accidental exposure of webhook data without integrity verification.
