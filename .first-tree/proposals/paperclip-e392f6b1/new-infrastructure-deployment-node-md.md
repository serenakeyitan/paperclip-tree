---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: PR increases Node.js keepAliveTimeout to prevent 502 errors behind reverse proxies — a production deployment concern not captured in the deployment node.
---
### Node keepAliveTimeout Behind Reverse Proxies

The server's `keepAliveTimeout` is increased above the default to prevent 502 errors when running behind reverse proxies (e.g., nginx, cloud load balancers). The default Node.js timeout can close connections before the proxy expects, causing intermittent failures under load.
