---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Image attachments displayed as a square-cropped gallery grid is a new UI pattern with no existing node covering attachment display conventions.
---
# Image Attachment Display

How image attachments are rendered in issue views — display format, cropping, and layout conventions.

## Key Decisions

### Square-Cropped Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than as a list of filenames or inline full-size images. This provides a compact visual preview of all attached images without consuming excessive vertical space.

**Rationale:** Issues frequently accumulate multiple image attachments (screenshots, diagrams, design mockups). A filename list provides no visual information; inline full-size images dominate the page. The gallery grid balances visual preview with space efficiency.

Source: PR `Display image attachments as square-cropped gallery grid`
