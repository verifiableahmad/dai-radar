# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static, hand-curated reference site (`dai.misbar.tech`) tracking how
decentralized-AI / crypto-ML systems (`systems/*.md`) relate to the
academic papers they're actually built on (`foundations/*.md`), plus a
daily "watch" log of what changed. There is no app server and no build
pipeline beyond one script: the markdown files are the database, and
`index.json` is a generated read-only projection of them for `index.html`
to consume client-side.

## Commands

- Regenerate `index.json` after editing anything in `systems/`,
  `foundations/`, or `watch/log/`:
  ```
  python3 scripts/build_index.py
  ```
  No dependencies beyond the Python standard library. There is no test
  suite, linter, or other build step in this repo.
- Serve locally to check `index.html` rendering: any static file server
  (e.g. `python3 -m http.server`) pointed at the repo root. PostHog
  analytics (snippet in `index.html`'s `<head>`, same project as
  misbar.tech) stays off on localhost / 127.0.0.1 and `file:`.

## Content architecture

- **`systems/*.md`**: one file per real, shipping (or shipping-adjacent)
  system (e.g. EZKL, Bittensor, Akash). Each has YAML frontmatter
  (`name`, `field`, `category`, `foundation`, `last_updated`,
  `confidence`, `website`) and a fixed set of `##` sections (e.g. "What it does",
  "Architecture summary", "Built on (foundational lineage)", "Property
  matrix", "Known limitations"). The `foundation:` frontmatter key is
  the **authoritative** link to a `foundations/*.md` file; it is never
  inferred from the `field` name, because multiple systems can share a
  field while citing different (or no) paper lineage. `category:` is a
  coarser grouping (one of the ids in `CATEGORIES` in `index.html`) that
  drives the radar's sectors and the Systems page groups; a system with a
  missing or unknown category shows up under "Uncategorized".
- **"Comparison axes"**: a `##` section right after "Property matrix",
  holding a `| Axis | Class | Detail | Source |` table with exactly six
  rows (Correctness, Trust rests on, Confidentiality, Lineage, Maturity,
  On-chain). It's what the Compare view lines up across fields, since
  property matrices are field-specific. `Class` must come from that
  axis's vocabulary in `COMPARE_AXES` in `index.html` (or NONE FOUND /
  OPEN QUESTION / NOT COVERED). Each row only restates a claim the file
  already makes, with that claim's source and tier ("inferred" in the
  Source cell marks it inferred); NOT COVERED means the file's sources
  are silent, not a negative finding. A file without the section still
  renders and shows "no profile" in Compare.
- **`foundations/*.md`**: one file per foundational paper/technique
  (e.g. PLONK, Halo2 lineage, secure aggregation). Same
  frontmatter+sections pattern, cited by one or more systems.
- **Grounding discipline**: claims are tagged as "grounded" (directly
  sourced/quoted) vs "inferred," and lineage is only asserted when a
  real citation chain exists (e.g. EZKL → names Halo2 → Halo2 docs cite
  PLONK). A plausible-but-unverified link (e.g. Halo2 ← original "Halo"
  paper) is explicitly dropped rather than asserted. Preserve this
  distinction when editing: don't upgrade "inferred" to "grounded"
  without an actual source, and don't delete the caveats that justify
  an inferred claim.
- **`watch/`**: the daily tracking mechanism:
  - `watch/checkpoint.json` holds the schedule and the sources polled
    (IACR ePrint, arXiv cs.CR/cs.LG/cs.DC, tracked systems' own repos).
  - `watch/log/<date>.md` (and `<date>-daily.md` for same-day re-runs)
    are dated entries with frontmatter stats (`new_items_found`,
    `matrix_impacting`, `filed_related_only`) and a "What happened"
    section. Most runs are expected to find nothing; that's the
    normal/healthy outcome, not a sign the watch is broken.
- **`scripts/build_index.py`**: parses the frontmatter + `##` sections
  (including markdown tables and lists within them) out of every file
  in `systems/`, `foundations/`, and `watch/log/`, and writes the
  combined result to `index.json`. `index.html` fetches `index.json` at
  runtime and renders everything client-side (vanilla JS, no framework,
  no bundler). The same run also writes `llms.txt` (index, linking raw
  GitHub copies, since Pages' Jekyll doesn't serve the `.md` sources) and
  `llms-full.txt` (every source file inlined), which the "Ask Claude" /
  "Ask ChatGPT" links on the landing page point assistants at.

## Editing conventions

- When adding or updating a system/foundation file, keep the section
  headings consistent with sibling files in the same directory (the
  parser in `build_index.py` keys off `##` headings and expects
  tables/lists in a specific markdown shape; check an existing file in
  the same directory before inventing a new section format).
- Always re-run `scripts/build_index.py` after editing markdown source
  so `index.json` stays in sync; it's a committed, generated artifact,
  not derived at request time.

## Writing style

- This is a reference tool, not a pitch. Write plain descriptions of what
  something is or does; no taglines, slogans, or "not X, but Y" one-liners.
- No em dashes (—) anywhere in site copy or content files. Use a comma,
  colon, semicolon, parentheses, or a new sentence. The one exception is a
  direct quotation, which stays verbatim.
- An empty Comparison-axes Detail cell is left blank, not filled with a dash.
