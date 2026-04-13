---
title: "Security"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Security

Security conventions and constraints for the Paperclip platform.

## Key Decisions

### No Hardcoded Secret Fallbacks

Auth initialization must fail loudly at startup if `BETTER_AUTH_SECRET` is missing — never fall back to a hardcoded value. This is defense-in-depth independent of Docker Compose enforcement. A missing secret is a deployment error that must be caught immediately, not papered over with a default that could be exploited.

### Bearer Token Redaction in Logs

Bearer tokens are redacted from all server log output. Authorization headers and token values must never appear in plaintext in logs, regardless of log level. This prevents credential leakage through log aggregation systems, error reports, or debug output. The redaction is applied at the logging layer so individual route handlers cannot accidentally bypass it.

### Defense in Depth

Security controls are layered — secrets validation at startup, token redaction at the log layer, forbidden-token checks in CI (`scripts/check-forbidden-tokens.mjs`), capability-dropped Docker containers for untrusted review, and governance enforcement at the server layer. No single layer is assumed sufficient.
