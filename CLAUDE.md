# Breadboard

`Breadboard.html` is a single-file web app (no build step, no dependencies) for Shape Up style
breadboarding. It is published by GitHub Pages at
https://mattstratford.github.io/pdm-hacks/Breadboard.html. Pushing to the Pages branch publishes it.

The owner is a product manager learning to build, not a programmer. Explain changes in plain
language and say what was and was not tested.

## What it does

- Left pane: a Markdown outline in a textarea. Right pane: a canvas "whiteboard" drawn from it.
- A top-level bullet is a place. A bullet nested under a place is an affordance.
- `- Affordance -> Place` links an affordance to a place by name (case-insensitive).
  `Place > Affordance` targets one affordance. A bullet nested under an affordance is the place
  it leads to.
- Lines that are not bullets are notes and are not drawn. A `# Heading` on the first non-blank
  line is the board title.
- Layout mirrors the outline: top-level places stack in the left column, nested places sit one
  column to the right. Links never move anything.
- Connection lines are purple, routed on a grid (A*) around every entity and the title, and hop
  over each other where they cross.
- Text and Draw tools mark up the board. Marks are not saved in the Markdown file.
- Autocomplete after `->` lists places; the "Allow linking to affordances" checkbox adds affordances.
- Click the file name to rename it. Save writes Markdown; Export writes .jpg, .pdf, .docx, .opml, .md.

## Code map (all inside the one `<script>`)

1. state and constants
2. parse: `docTitle`, `bulletTree`, `buildGraph`
3. layout: `layout`, `makeRouter`
4. draw: `drawScene`, `drawRoute`, `paint`
5. editor keys and autocomplete (`acUpdate`)
6. markup tools
7. platform: all file reading and writing (replace this section for a native Mac port)
8. exporters: hand-written PDF and ZIP/DOCX writers, OPML
9. wiring

## Rules

- Keep it one self-contained file that works offline. No frameworks, no CDN scripts.
- Lines must never cross text.
- The same Markdown file must draw the same board for everyone; preferences may change the
  editor, not the drawing.
- Sections 2 and 3 are pure functions and can be tested in Node with a fake `ctx.measureText`.
  Drawing, the tools and exports need a browser.
