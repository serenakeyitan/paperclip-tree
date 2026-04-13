---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: adapters/claude-local/NODE.md
supersedes: adapters/claude-local/NODE.md
rationale: AWS Bedrock model selection support and session resume prompt-file skipping were added to the Claude adapter — new capabilities not in the current node.
---
The existing node should be updated to add:

- **AWS Bedrock support** — Users can select Bedrock-hosted models with region-prefixed model IDs. The adapter reuses a DIRECT_MODELS import for model validation and applies global region prefix matching.
- **Session resume refinement** — The adapter now skips `--append-system-prompt-file` on resumed sessions and gates instructions file I/O to fresh sessions only, preventing duplicate prompt injection on resume.
