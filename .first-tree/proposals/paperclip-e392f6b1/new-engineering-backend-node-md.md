---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR increases Node keepAliveTimeout to prevent 502s behind reverse proxies (e2962e6) — this is a production-relevant server configuration decision not captured in the backend node.
---
### Keep-Alive Timeout for Reverse Proxies

The Node.js HTTP server's `keepAliveTimeout` is set higher than default to prevent 502 errors when running behind reverse proxies (e.g., nginx, cloud load balancers). The default Node timeout can be shorter than the proxy's idle timeout, causing the proxy to send requests on connections the server has already closed.
