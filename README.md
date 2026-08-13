# Pitt Civil Engineering — Prerequisite Tree

An interactive prerequisite tree for the University of Pittsburgh BSE Civil
Engineering curriculum (Swanson School of Engineering).

Single-file static web app: pure HTML/CSS/vanilla JS + SVG in `index.html`.
No build step, no dependencies beyond two Google Fonts loaded via CDN.

## Features

- Click a class to highlight it and its full prerequisite path; clicks stack
  for multi-path selection ("Reset selections" clears).
- Search with two modes: matches + their prereqs, or matches only.
- 8 paint colors + a default chip — arm a chip, click nodes to paint.
- Toggleable color key with editable names.
- Draggable nodes (edges follow), background pan, wheel/pinch zoom,
  Fit tree and Reset layout buttons.
- ENGR 0141 ↔ CEE 1105 rendered as a dashed CO-REQ edge.

## Editing course data

All course data lives inline in the `<script>` block of `index.html`:

- **`NODES`** — one entry per course: `[term, id, code, name, x]`
  (term number 1–8, unique id, course code, course name, x-position on the
  canvas).
- **`EDGES`** — one entry per prerequisite link: `[prereqId, courseId]`,
  with an optional third `'coreq'` flag for co-requisites.

Prerequisite highlighting is computed from `EDGES` automatically — nothing
else needs updating when courses change.

### Data sources for curriculum updates

- First-year: engineering.pitt.edu/first-year/academic/first-year-integrated-curriculum/
- CEE curriculum: engineering.pitt.edu/departments/civil-environmental/undergraduate/civil-engineering/
  (term tables load from the Pitt Acalog catalog at catalog.upp.pitt.edu —
  search "Civil Engineering" there for the raw program listing)

## Hosting / deployment

The site is hosted on Cloudflare Pages, connected to this GitHub repo.
Every push deploys automatically — no build command, output directory `/`.

## Conventions

- Keep it a single `index.html` — no build tooling, no framework migration.
- Commit with clear messages and push after each working change so
  Cloudflare redeploys.
