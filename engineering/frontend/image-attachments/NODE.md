---
title: "Image Attachment Display"
owners: [plind-dm]
---

# Image Attachment Display

How image attachments are rendered in issue views — display format, cropping, and layout conventions.

## Key Decisions

### Square-Cropped Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than as a list of filenames or inline full-size images. This provides a compact visual preview of all attached images without consuming excessive vertical space.

**Rationale:** Issues frequently accumulate multiple image attachments (screenshots, diagrams, design mockups). A filename list provides no visual information; inline full-size images dominate the page. The gallery grid balances visual preview with space efficiency.

Source: PR `Display image attachments as square-cropped gallery grid`
