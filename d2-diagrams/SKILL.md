---
name: d2-diagrams
description: Use when creating or editing D2 diagrams for docs — authoring, rendering to SVG, embedding in Markdown, and verifying they render on GitHub.
---

# D2 Diagrams

Author diagrams as `.d2` source, render to `.svg`, commit both, embed the SVG in Markdown as an image. The `.d2` is the editable source; the `.svg` is what people see.

## Setup

`d2` must be installed: `brew install d2` (or `go install oss.terrastruct.com/d2@latest`). Check with `which d2`.

## Render

```
d2 path/to/diagram.d2 path/to/diagram.svg
```

Editing a diagram means editing the `.d2` and **re-rendering** — never hand-edit the `.svg`. Commit the `.d2` and `.svg` together so they never drift.

## Embed in Markdown

Use an image embed, not a link — GitHub renders `![...]()` inline but shows `[...]()` as clickable text only:

```markdown
![Alt text](../diagrams/diagram.svg)
```

## Verify before finishing

SVGs are large and text-heavy; don't trust that it "compiled". Render a raster preview and Read it as an image to catch clipping and layout problems:

```
qlmanage -t -s 1400 -o . diagram.svg   # macOS → diagram.svg.png
```

Read the `.png`, confirm the layout, then delete the previews (`rm *.svg.png`) — they are not committed.

## Gotchas that will bite

- **Titles/notes clip.** Markdown blocks (`|md # Title |`) wrap at a fixed width and get cut off. Use a text shape instead:
  ```
  title: My Diagram Title {
    near: top-center
    shape: text
    style.font-size: 24
    style.bold: true
  }
  ```
- **Keep label text short.** Long multi-line labels overflow their box. Split into child shapes or shorten.
- **Sequence diagrams:** first line is `shape: sequence_diagram`. Group related steps in a named block (`connect: Step group { ... }`) to get labelled lifespans.
- **Escape newlines in labels** with `\n`, not real line breaks inside quotes.

## Checklist

- [ ] `.d2` renders with no errors
- [ ] Previewed the `.svg` as an image — nothing clipped or overlapping
- [ ] Embedded with `![...]()`, relative path correct
- [ ] `.d2` + `.svg` both committed, previews deleted
