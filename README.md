# Pitt Prerequisite Trees

Interactive prerequisite trees for University of Pittsburgh majors, selectable
via a dropdown in the ribbon:

- **Civil Engineering** (BSE, Swanson School of Engineering)
- **Neuroscience** (BS, Dietrich School of Arts & Sciences)

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
- Co-requisites rendered as dashed CO-REQ edges (e.g. ENGR 0141 ↔ CEE 1105,
  CHEM 0320 ↔ CHEM 0345).
- **Auto-save**: selections, painted colors, legend names, dragged node
  positions, and the last-viewed major persist across page reloads via
  `localStorage` (key `pittPrereqTree.v1`), kept separately per major.
  Clearing the browser's site data resets everything.

## Editing course data

All course data lives inline in the `MAJORS` object in the `<script>` block
of `index.html`. Each major has:

- **`title` / `school`** — shown in the ribbon header.
- **`terms`** — term number → row label.
- **`NODES`** — one entry per course: `[term, id, code, name, x]`
  (term number 1–8, unique id, course code, course name, x-position on the
  canvas).
- **`EDGES`** — one entry per prerequisite link: `[prereqId, courseId]`,
  with an optional third `'coreq'` flag for co-requisites.

Prerequisite highlighting is computed from `EDGES` automatically — nothing
else needs updating when courses change. To add another major, add a new
entry to `MAJORS` and a matching `<option>` to the `#majorSel` dropdown.

### Data sources for curriculum updates

Civil Engineering:

- First-year: engineering.pitt.edu/first-year/academic/first-year-integrated-curriculum/
- CEE curriculum: engineering.pitt.edu/departments/civil-environmental/undergraduate/civil-engineering/
  (term tables load from the Pitt Acalog catalog at catalog.upp.pitt.edu —
  search "Civil Engineering" there for the raw program listing)

Neuroscience:

- Major requirements: neuroscience.pitt.edu/programs/undergraduate-program/major-requirements
- Major sheet (plan of study): asundergrad.pitt.edu/academics/majors/neuroscience-bs
- Course prerequisites: catalog.upp.pitt.edu (search course codes, e.g.
  BIOSC 1000, NROSCI 1250, CHEM 0345)

## Hosting / deployment

The site is hosted on Cloudflare Pages, connected to this GitHub repo.
Every push deploys automatically — no build command, output directory `/`.

## Conventions

- Keep it a single `index.html` — no build tooling, no framework migration.
- Commit with clear messages and push after each working change so
  Cloudflare redeploys.
