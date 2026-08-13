# Handoff: Pitt CE Prerequisite Tree → GitHub + Cloudflare

Give this file to Claude Code along with `pitt-ce-prereq-tree.html`. Paste the
"Brief for Claude Code" section as your first message, or just drop this file
in the project folder and say "read HANDOFF.md and do it."

---

## One-time setup YOU need before Claude Code can act

Claude Code runs shell commands but uses your credentials. Have these ready:

1. **GitHub CLI authenticated** — run `gh auth login` once in your terminal
   (Claude Code can walk you through it if it's not done). Verify with
   `gh auth status`.
2. **Cloudflare account** — free tier is fine (Cloudflare Pages free plan
   covers static sites like this one).
3. **Cloudflare auth for Wrangler** — Claude Code will run
   `npx wrangler login` (opens a browser to authorize) OR you can create an
   API token at dash.cloudflare.com → My Profile → API Tokens ("Edit
   Cloudflare Workers" template) and set it as `CLOUDFLARE_API_TOKEN`.
4. **Node.js installed** — required for both Claude Code and Wrangler.

---

## Brief for Claude Code

**Project:** A single-file static web app, `pitt-ce-prereq-tree.html` — an
interactive prerequisite tree for the University of Pittsburgh BSE Civil
Engineering curriculum. Pure HTML/CSS/vanilla JS + SVG, no build step, no
dependencies beyond two Google Fonts loaded via CDN. All course data and
prerequisite edges are inline in the `NODES` and `EDGES` arrays in the
script block.

**Task 1 — Repo setup:**
- Create a project folder (suggest `pitt-ce-prereq-tree/`).
- Copy `pitt-ce-prereq-tree.html` in and rename it `index.html` so it serves
  at the site root.
- Add a short `README.md` (what it is, how to edit course data via the
  NODES/EDGES arrays, note that it deploys automatically on push).
- `git init`, initial commit, then create and push a GitHub repo:
  `gh repo create pitt-ce-prereq-tree --public --source=. --push`
  (make it private instead if I say so).

**Task 2 — Cloudflare hosting (pick per my preference):**

*Option A — Git-connected Pages project (preferred: auto-deploys on every
push, good for iterating):*
- The repo connection step happens in the Cloudflare dashboard, which you
  can't click through — walk me through it: dash.cloudflare.com → Workers &
  Pages → Create → Pages → Connect to Git → select the repo. Build command:
  none. Build output directory: `/`.
- After I connect it, every `git push` you make deploys automatically.

*Option B — Direct upload from the CLI (no dashboard clicking, but deploys
only when explicitly run):*
- `npx wrangler pages project create pitt-ce-prereq-tree`
- `npx wrangler pages deploy . --project-name=pitt-ce-prereq-tree`
- Re-run the deploy command after each change.

**Verify:** After deploy, give me the `*.pages.dev` URL and confirm the page
loads (fonts, zoom, node dragging).

**Ongoing conventions for this project:**
- Keep it a single `index.html` — no build tooling, no framework migration
  unless I ask.
- Course data changes = edit the `NODES` (term, id, code, name, x-position)
  and `EDGES` (prereq → course, optional `'coreq'` flag) arrays. Prereq
  highlighting is computed from EDGES automatically; nothing else needs
  updating when courses change.
- Commit with clear messages and push after each working change so
  Cloudflare redeploys.

---

## Current feature set (so Claude Code doesn't reinvent it)

- Click a class → highlights it + all ancestors (full prereq path); clicks
  stack for multi-path selection; "Reset selections" clears.
- Search box with two ribbon modes: matches + their prereqs, or matches only.
- 8 paint colors + default chip; arm a chip, click nodes to paint.
- Color key overlay, toggleable, names editable when "Edit names" is on.
- Ribbon tabs toggle each toolbar section (Search / Colors / Key / View).
- Nodes draggable with edges following; background pan; wheel zoom + pinch;
  Fit tree and Reset layout buttons.
- ENGR 0141 ↔ CEE 1105 rendered as a dashed CO-REQ edge.

## Data sources (for future curriculum updates)

- First-year: engineering.pitt.edu/first-year/academic/first-year-integrated-curriculum/
- CEE curriculum: engineering.pitt.edu/departments/civil-environmental/undergraduate/civil-engineering/
  (the term tables load from the Pitt Acalog catalog at catalog.upp.pitt.edu —
  search "Civil Engineering" there for the raw program listing)
