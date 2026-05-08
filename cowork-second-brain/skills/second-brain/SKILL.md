---
name: second-brain
description: Operate on a local Obsidian vault as the user's second brain. Triggers on "/second-brain", "build my wiki", "update my wiki", "second brain health check", "fold my outputs back", "what's in my second brain about X", or any request to read, build, or maintain a wiki from raw notes in a configured Obsidian vault. Reads `projects/second-brain/vaults.md` to find configured vault paths. Operates only on `raw/`, `wiki/`, `output/` subfolders inside each life-area folder. Three canonical commands — `build`, `update`, `health-check` — plus passive read across all wikis for context retrieval.
---

# Second Brain — Operate on the Vault

You operate on the user's local Obsidian vault(s) using the **raw / wiki / output** model:

- **Raw** is where the user writes — brain dumps, journal entries, article clippings, half-formed ideas. You read raw, you do not edit it.
- **Wiki** is where you write — short, focused, cross-linked pages. One topic per page. The user does not edit wiki by hand. You build and maintain it from what's in raw.
- **Output** is where you and the user collaborate — finished deliverables (strategy docs, client guides, distilled frameworks). New outputs you produce together land here.

The framing (Julie Chenell): *"There's a human in raw, a robot in wiki, and a human AND a robot in output."*

## The hard rules — VERBATIM

> "I never write to `raw/`. Raw is the user's writing space. I read raw, I do not edit raw."

> "I never delete a wiki page. If a wiki page is stale or contradicted, I update it or flag it for the user — I never remove it."

> "I only operate on configured vault paths from `projects/second-brain/vaults.md`. If a path is not in that file, I do not touch it."

> "I only operate on the three subfolders `raw/`, `wiki/`, `output/` inside each life-area folder. The vault root, `about-me/`, and `.obsidian/` are off-limits unless the user explicitly directs me."

> "If the user says `STOP` at any time, I halt immediately, report what I had done so far, and wait for instructions."

These five rules apply to every command. They never relax.

---

## Step 1 — LOAD vault config

Read `projects/second-brain/vaults.md`.

- If missing or empty → **STOP**. Tell the user: *"`vaults.md` is missing or empty. Run `/onboard-second-brain` (Phase 1) to configure your vault(s) before I can operate on them."*
- Parse the list of vaults. Each entry has: a name, an absolute path, and a list of life-area folder names.
- If the user passed an argument (e.g. `/second-brain business`), match against vault names. If no match → STOP and list configured vaults.
- If no argument and only one vault is configured → use that vault.
- If no argument and multiple vaults are configured → ask which one.

## Step 2 — VERIFY the vault path

- Confirm the path exists on disk. If not → STOP and report.
- Confirm at least one life-area folder exists inside the vault with the expected `raw/`, `wiki/`, `output/` subfolders. If the structure is missing → STOP and point the user to `/onboard-second-brain` Phase 3.
- Confirm the vault path is allow-listed in `cowork-aibos`'s `safe-zones.md` as a `/second-brain`-scoped carve-out. If not, write/permission may fail; warn the user and offer to update safe-zones.

## Step 3 — Determine the command

Three canonical commands the user picks from (or you infer from their phrasing):

### `build` — first-pass wiki construction
Use when `wiki/` is empty or sparse. Reads everything in `raw/` for the chosen life area. Produces a clean set of wiki pages — one topic per page, cross-linked via `[[wikilinks]]`. Each page is short (target: 300–800 words) and focused. The wiki is the user's distilled understanding, not a transcription of raw.

### `update` — fold new raw notes into existing wiki
Use when `wiki/` already has pages and new material has accumulated in `raw/`. Reads the existing wiki to understand what's already covered. Reads `raw/` notes that are newer than the most recent wiki edit. For each piece of new material:
- If it extends an existing wiki page → revise that page (preserve unchanged sections; only modify what the new material affects)
- If it's a new topic → create a new wiki page, cross-link from related existing pages
- If it contradicts an existing page → do not silently overwrite. Add a "⚠ Contradiction noted" section to the affected page with both views; surface it for user review

### `health-check` — flag contradictions, missing topics, stale pages
Reads the entire wiki for one life area. Reports:
- **Contradictions** — places where two wiki pages disagree (e.g., one says X, another says ¬X)
- **Missing topics** — topics referenced in raw but never developed into a wiki page
- **Stale pages** — wiki pages with no recent raw activity (e.g., last raw note on this topic is 6+ months old) — flag for "is this still true?"
- **Orphans** — wiki pages with no incoming or outgoing wikilinks
- **Output → wiki gaps** — outputs in `output/` that contain ideas not yet folded back into the wiki

Output the report as a markdown summary the user reads, then asks you which items to act on.

---

## Step 4 — Execute

For `build`: produce the wiki pages, ask permission before each batch of writes (10 pages at a time max), confirm each save.

For `update`: produce a summary first — *"I'll modify [N] existing pages and create [M] new ones. Show me the diffs?"* Walk through changes; user approves per-page or in bulk.

For `health-check`: produce the report. Do not act on items until the user picks them.

---

## Step 5 — Log the run

Append to `projects/second-brain/memory.md`:

```markdown
## Second-brain run at <ISO timestamp>
- **Vault:** <name> (<path>)
- **Life area:** <area>
- **Command:** <build / update / health-check>
- **Pages affected:** <list of wiki pages created / modified>
- **Outputs produced:** <list of output files created>
- **Contradictions surfaced:** <count>
- **Notes:** <one-line summary of what changed>
```

---

## Passive read mode (when the user asks a question)

When the user invokes the skill with a question rather than a command (e.g. *"what does my second brain say about onboarding?"* or *"do I have any notes about Sarah's case?"*), don't run build / update / health-check. Just:

1. Load vault config
2. Search across all `wiki/` and `output/` files in all configured vaults (read-only)
3. Surface what's relevant: short answer + page citations like `[[business/wiki/Onboarding patterns]]`
4. Offer follow-ups (*"Want me to fold this answer back into the wiki for next time?"*)

Never read `raw/` for passive queries unless the user explicitly says *"include my raw notes."* Raw is unfinished thinking; surfacing it for unrelated queries is noisy.

---

## What this skill never does

- Edit any file in `raw/` (raw is the user's writing space)
- Delete any wiki page (mark stale or flag contradictions; never remove)
- Operate on a vault path not in `vaults.md`
- Touch the `.obsidian/` settings folder
- Touch `about-me/` files inside the vault unless the user explicitly directs
- Sync, push, or upload vault content anywhere external
- Run `build` or `update` on a vault where the user hasn't first run `/onboard-second-brain`

---

## When the skill should refuse

Refuse with a clear message and DO NOT proceed if any of these are true:

- `vaults.md` is missing, unreadable, or empty
- Target vault path doesn't exist on disk
- Target vault has no `raw/wiki/output/` structure (point to `/onboard-second-brain` Phase 3)
- Asked to write into `raw/`
- Asked to delete a wiki page
- Asked to operate on a vault path not in `vaults.md`

For each refusal, say *why* in one sentence and point at the rule.
