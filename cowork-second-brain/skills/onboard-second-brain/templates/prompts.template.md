# The Three Canonical Prompts

These are the three operations the `/second-brain` skill performs. They're encoded into the operational skill (you don't type them as raw prompts — you invoke the skill). Listed here for understanding and for future SOP / curriculum reference.

---

## 1. `build` — first-pass wiki construction

**When to use:** wiki/ is empty or sparse. First run after Phase 5 of onboarding, or after starting a new life area.

**What it does:**
1. Loads the configured vault from `vaults.md` and the target life area
2. Reads every file in `<vault>/<life-area>/raw/`
3. Reads `<vault>/about-me/` for context and tone
4. Identifies distinct topics in the raw material
5. Constructs one wiki page per topic, with cross-links via `[[wikilinks]]`
6. Each page is short (target: 300–800 words) and focused
7. Shows drafts to the user before saving
8. Saves approved pages to `<vault>/<life-area>/wiki/`

**Hard rules:**
- The wiki is distilled understanding, NOT a transcription of raw
- One topic per page; if a topic is too broad, split it
- Cross-link aggressively via `[[wikilinks]]` — that's how Obsidian's graph view becomes useful

---

## 2. `update` — fold new raw into existing wiki

**When to use:** weekly (or whenever 2–3 new raw notes have accumulated). Existing wiki has pages.

**What it does:**
1. Loads vault and life area
2. Reads existing wiki to understand what's already covered
3. Reads raw notes that are newer than the most recent wiki edit (use file mtime)
4. For each new piece of raw material:
   - **If it extends an existing wiki page:** revise that page (preserve unchanged sections; only modify what the new material affects)
   - **If it's a new topic:** create a new wiki page; cross-link from related existing pages
   - **If it contradicts an existing page:** do not silently overwrite. Add a "⚠ Contradiction noted" section with both views; flag for user review
5. Shows diffs before saving
6. User approves per-page or in bulk

**Hard rules:**
- Never silently overwrite a wiki page based on a single new raw note. Surface contradictions.
- Preserve unchanged content. `update` is incremental, not regenerative.
- Don't update wiki pages that have no new raw input — leave them alone.

---

## 3. `health-check` — flag contradictions, missing topics, stale pages

**When to use:** monthly. After 4+ weeks of `update` runs.

**What it does:**
1. Loads vault and life area
2. Reads the entire wiki
3. Reports:
   - **Contradictions** — places where two wiki pages disagree (e.g., one says X, another says ¬X)
   - **Missing topics** — topics referenced in raw but never developed into a wiki page
   - **Stale pages** — wiki pages with no recent raw activity (last raw note on this topic is 6+ months old)
   - **Orphans** — wiki pages with no incoming or outgoing wikilinks
   - **Output → wiki gaps** — outputs in `output/` that contain ideas not yet folded back into the wiki
4. Outputs a markdown report
5. Asks the user which items to act on

**Hard rules:**
- `health-check` only reports. It NEVER acts on findings without the user picking which to address.
- Don't auto-delete stale pages. Stale ≠ wrong; they may still be true. Flag, don't remove.
- The report is a conversation starter, not a to-do list. The user decides what's worth fixing.

---

## Optional fourth prompt: `fold-back`

**When to use:** ad-hoc. After a Cowork session produces something great in `output/`, the user wants the underlying ideas absorbed into the wiki.

**What it does:**
1. Reads a specific file in `<vault>/<life-area>/output/`
2. Identifies the core ideas / insights / frameworks the output contains
3. For each idea: extends a relevant wiki page OR creates a new one (same logic as `update`)
4. Shows diffs; user approves
5. Logs the fold-back in `memory.md`

`fold-back` is how outputs (which are usually one-off deliverables) become permanent knowledge in the wiki.
