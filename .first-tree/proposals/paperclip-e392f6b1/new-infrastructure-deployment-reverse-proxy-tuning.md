---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node covering Node.js HTTP server timeout tuning for reverse proxy deployments; the keepAliveTimeout fix prevents 502s when running behind ALB/nginx but this operational constraint is not captured anywhere.
---
# Reverse Proxy Tuning

Operational constraints for running the Paperclip server behind reverse proxies (AWS ALB, nginx, Cloudflare, etc.).

## keepAliveTimeout

Node.js's default `keepAliveTimeout` (5 seconds) is shorter than most reverse proxies' idle timeout (e.g., AWS ALB defaults to 60 seconds). When the proxy holds a connection open longer than the server's keepAliveTimeout, the server closes the socket while the proxy still considers it active, causing intermittent **502 Bad Gateway** errors.

The server sets `keepAliveTimeout` to a value **higher** than the upstream proxy's idle timeout. This is configured in the HTTP server bootstrap, not via environment variable, because it must always exceed the proxy default — it is not a user-tunable knob.

## Key Decision

Server-side keepAliveTimeout must always exceed the reverse proxy's idle timeout. This is a correctness constraint, not a performance preference. Any change to the server's listen configuration must preserve this invariant.
