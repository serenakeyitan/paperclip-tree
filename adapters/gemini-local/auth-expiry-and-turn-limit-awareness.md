---
title: "Auth Expiry And Turn Limit Awareness"
owners: [cryppadotta, serenakeyitan]
---

# Auth Expiry And Turn Limit Awareness

Gemini's CLI failure modes are operationally important because expired auth and
model turn limits can look like generic command failures if the adapter does
not detect them explicitly.

## Decision

Make Gemini-specific auth-expiry and turn-limit detection first-class adapter
behaviors instead of letting those cases collapse into generic execution
errors.

## Why

The control plane needs to know whether an agent truly failed, needs human
re-authentication, or simply reached a runtime cap that might allow a
continuation strategy. Those distinctions are runtime-specific and belong at
the adapter edge.

## Implications

- Paperclip can surface a more actionable operator message when Google auth has
  expired.
- Server-side continuation or retry logic can treat turn caps differently from
  unexpected process failures.
- Gemini-specific helper logic remains adapter-owned rather than spreading
  provider conditionals through backend orchestration code.
- Environment tests for Gemini are about operational readiness, not just
  whether the binary exists on disk.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
