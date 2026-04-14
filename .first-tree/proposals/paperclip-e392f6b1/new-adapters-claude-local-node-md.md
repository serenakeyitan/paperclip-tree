---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The Claude Local Adapter node delegates credentials to Claude CLI's credential store but says nothing about AWS Bedrock as an alternative auth/provider pathway — this PR adds Bedrock auth support which is a new capability dimension.
---
### AWS Bedrock Auth Support

The Claude Local Adapter supports AWS Bedrock as an alternative provider backend. When Bedrock auth is configured, the adapter passes the necessary AWS credentials and region configuration to the Claude CLI process, allowing Paperclip-managed agents to use Claude models hosted on AWS Bedrock instead of the Anthropic API directly. This is relevant for deployments that require data residency, VPC-scoped access, or AWS-native billing.

**Key detail:** Bedrock auth is opt-in and coexists with the default Anthropic API credential path. The adapter detects which auth mode is configured and adjusts the CLI invocation accordingly.
