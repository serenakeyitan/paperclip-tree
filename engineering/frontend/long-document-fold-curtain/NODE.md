---
title: "Long Document Fold Curtain"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/NODE.md, engineering/frontend/issue-document-freshness/NODE.md, engineering/frontend/issue-editor-reliability/NODE.md, engineering/frontend/issue-thread-ux/NODE.md]
---

Long markdown display surfaces in the issue UI clip overly tall content behind a fade curtain that the reader can expand. The behavior lives in the reusable `FoldCurtain` component (`ui/src/components/FoldCurtain.tsx`) and is opted into by callers rather than applied globally.

## Key Decisions

### Responsive Collapsed Height
The curtain collapses to 420px on desktop and 320px below the 640px viewport breakpoint, with a 72px fade region and a ~220ms expand transition. Callers can override `collapsedHeight`, but the responsive default keeps long descriptions and documents from dominating the viewport on mobile without bespoke per-call tuning.

### Activation Buffer Avoids Pointless Curtains
The curtain only engages when natural content height exceeds `collapsedHeight + activationBuffer`. Short content renders unchanged, so the More/Less control never appears for content that already fits — preventing visual noise on small descriptions.

### Opt-In via `foldable` on InlineEditor
`InlineEditor` accepts a `foldable` prop (default `false`) that wraps display-mode `MarkdownBody` in `FoldCurtain`. Edit mode is never curtained — only the rendered preview. The issue detail description editor passes `foldable` (`ui/src/pages/IssueDetail.tsx`); the issue documents section uses `renderFoldableBody` for legacy plan documents and conflict server-document previews (`ui/src/components/IssueDocumentsSection.tsx`). Other inline editors keep the previous unclipped behavior.

### Display-Only, Not a Data Concern
The fold curtain is purely a presentation wrapper around `MarkdownBody`. It does not change document storage, revision tracking, or thread interaction model — issue document freshness and editor reliability conventions still apply unchanged.
