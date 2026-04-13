---
title: "Agent JWT Authentication"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Agent JWT Authentication

Agents authenticate to the Paperclip server using JWT tokens, separate from the BetterAuth session-based authentication used by human board members.

## Secret Configuration

Agent JWTs are signed and verified using `PAPERCLIP_AGENT_JWT_SECRET`. When this variable is not set, the system falls back to `BETTER_AUTH_SECRET` as the signing key. This fallback simplifies local development and single-secret deployments while allowing production environments to isolate agent auth credentials from the human session auth secret.

## Key Decisions

### Separate Secret for Agent Auth

Agent JWT signing uses its own dedicated secret (`PAPERCLIP_AGENT_JWT_SECRET`) rather than always sharing `BETTER_AUTH_SECRET`. This allows operators to rotate agent credentials independently of human session auth, which is important for production deployments where agent API keys may be distributed across multiple machines.

### Graceful Fallback to BETTER_AUTH_SECRET

When `PAPERCLIP_AGENT_JWT_SECRET` is absent, the system silently falls back to `BETTER_AUTH_SECRET`. This keeps the minimal-config local development experience simple (one secret) while supporting secret separation in production. The fallback is intentional, not a bug — it means every deployment that configures BetterAuth automatically has working agent auth without additional configuration.
