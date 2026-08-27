---
title: "Backend Environment Variables"
owners: [bingran-you, cryppadotta, serenakeyitan, cpfarhood]
---

# Backend Environment Variables

Conventions for how the backend server (`/server`) reads environment variables at startup, and how externally-provided values interact with auto-derived defaults. Reference docs live in `docs/deploy/environment-variables.md`.

## Respect Externally Set Values

When the process environment already provides a value for a variable the server would otherwise derive (for example `PAPERCLIP_API_URL`), the server must use the externally set value rather than overwriting it with a derived default. This matters for deployments behind reverse proxies, custom domains, or orchestrators (Docker, Fly, Render, etc.) where the externally advertised URL differs from what the server could infer from its own bind address and port.

The rule is: derive a default only when the variable is unset or empty. Never clobber an explicit operator-provided value. This preserves operator intent and keeps webhook callbacks, adapter registration, and client-facing URLs consistent with the deployment's actual public address.

## Startup Feedback

The startup feedback log (exercised by `server/src/__tests__/server-startup-feedback-export.test.ts`) surfaces the resolved values of key environment variables so operators can verify at boot whether their overrides took effect. When adding a new env var with derivation logic, extend the startup feedback to echo the final resolved value and note whether it came from the environment or a derived default.
