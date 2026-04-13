---
title: ""
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Update the existing Backend Server NODE.md line 9 to read 'Express 5-based HTTP server' and add a key decision: **Express 5 routing syntax.** The server uses Express 5, which changes wildcard and path-parameter syntax from Express 4 (e.g., `*` becomes `*name`). Routes — especially catch-all handlers like the BetterAuth mount — must use Express 5 patterns. See PR #2936 for the migration fix.
