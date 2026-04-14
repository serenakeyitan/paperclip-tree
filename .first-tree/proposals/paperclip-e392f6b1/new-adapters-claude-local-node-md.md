---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title is 'add AWS Bedrock auth support on claude-local' — the existing node mentions Bedrock model IDs but not Bedrock authentication, which is a new auth pathway requiring configuration decisions.
---
### AWS Bedrock Authentication

The Claude local adapter supports AWS Bedrock as an alternative authentication and routing backend. When configured for Bedrock, the adapter authenticates via AWS credentials (IAM roles, access keys, or SSO) instead of Anthropic's native API key. This enables organizations that route Claude through their own AWS account to use Paperclip without exposing Anthropic API keys directly.

Add to Boundaries section:
- When using Bedrock auth, credential management shifts from Claude CLI's credential store to AWS credential chain (environment variables, profiles, IAM roles).
