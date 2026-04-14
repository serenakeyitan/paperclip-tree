---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The deployment node covers Docker, compose, and env vars but says nothing about Node server timeout tuning or reverse-proxy configuration — the keepAliveTimeout fix is a production deployment concern that belongs here.
---
### Reverse Proxy Timeout Tuning

When running behind a reverse proxy (nginx, AWS ALB, Cloudflare, etc.), Node's default `keepAliveTimeout` (5 s) can be shorter than the proxy's upstream keep-alive timeout, causing sporadic 502 errors. The server sets `keepAliveTimeout` to 65 s (and `headersTimeout` slightly higher) to ensure the server never closes idle connections before the proxy does. This is a standard Node.js production hardening step — see PR #3009.
