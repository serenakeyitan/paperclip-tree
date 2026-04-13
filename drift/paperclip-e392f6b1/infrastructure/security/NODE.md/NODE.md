---
title: "Security"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Security

Security conventions and constraints for the Paperclip platform.

## Key Decisions

### No Hardcoded Secret Fallbacks

Auth initialization must fail loudly at startup if `BETTER_AUTH_SECRET` is missing — never fall back to a hardcoded value. This is defense-in-depth independent of Docker Compose enforcement. A missing secret is a deployment error that must be caught immediately, not papered over with a default that could be exploited.
