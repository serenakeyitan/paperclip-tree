---
title: "Reverse Proxy Tuning"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Reverse Proxy Tuning

Operational constraints for running the Paperclip server behind reverse proxies (AWS ALB, nginx, Cloudflare, etc.).

## keepAliveTimeout

Node.js's default `keepAliveTimeout` (5 seconds) is shorter than most reverse proxies' idle timeout (e.g., AWS ALB defaults to 60 seconds). When the proxy holds a connection open longer than the server's keepAliveTimeout, the server closes the socket while the proxy still considers it active, causing intermittent **502 Bad Gateway** errors.

The server sets `keepAliveTimeout` to a value **higher** than the upstream proxy's idle timeout. This is configured in the HTTP server bootstrap, not via environment variable, because it must always exceed the proxy default — it is not a user-tunable knob.

## Key Decision

Server-side keepAliveTimeout must always exceed the reverse proxy's idle timeout. This is a correctness constraint, not a performance preference. Any change to the server's listen configuration must preserve this invariant.
