---
title: "Express 5 Routing"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Express 5 Routing

The Paperclip server runs Express 5 (not Express 4). This affects routing syntax across the entire backend.

## Key Decisions

### Wildcard Syntax Change

Express 5 changes wildcard/path-parameter syntax: `*` becomes `*name` (a named wildcard). All catch-all handlers (e.g., BetterAuth mount, SPA fallback) must use the Express 5 pattern. Using the old `*` syntax will cause runtime errors.

### Migration Constraint

Any new route or middleware mount must use Express 5 patterns. When copying route patterns from external documentation or examples, verify they use Express 5 syntax — most online resources still show Express 4.
