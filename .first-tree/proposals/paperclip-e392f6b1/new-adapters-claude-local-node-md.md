---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds AWS Bedrock auth support to the claude-local adapter, but the existing node only mentions Bedrock model IDs — it doesn't document Bedrock as an authentication/provider pathway or the auth configuration involved.
---
### AWS Bedrock Auth Support

The claude-local adapter supports **AWS Bedrock** as an alternative authentication and model provider pathway. When configured for Bedrock, the adapter authenticates via AWS credentials (IAM roles, access keys, or SSO) instead of Anthropic's direct API. This enables deployments in AWS-controlled environments where traffic must route through Bedrock.

**Rationale:** Enterprise deployments often require Bedrock for compliance, VPC isolation, or consolidated AWS billing. Supporting Bedrock auth natively means the adapter works in these environments without workarounds.

Add to Boundaries section:
- When using Bedrock auth, AWS credential management is delegated to the Claude CLI and standard AWS credential chain — Paperclip does not manage AWS credentials directly.
