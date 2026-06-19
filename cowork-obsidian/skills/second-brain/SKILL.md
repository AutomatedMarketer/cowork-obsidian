---
name: second-brain
description: "Operational skill for the cowork-obsidian vault. Five ops: build (first-time setup, run once), update (weekly, adds new raw notes to wiki), health-check (monthly, finds problems), fold-back (quarterly, folds outputs back into wiki), refresh-hot (monthly, regenerates wiki/hot.md hot cache for the v0.5 four-tier memory model). Mode-aware (silent in scaffold/overlay, asks per-write in external). Use-case picker (6 personas) when invoked with no args. Triggers on '/second-brain', 'build my wiki', 'update my wiki', 'refresh my wiki', 'refresh hot cache', 'regenerate hot.md', 'second brain health check', 'audit my second brain', 'fold my outputs back', 'fold output into wiki', 'what is in my second brain about X', 'run my second brain', 'process my raw notes', or any request to read, build, or maintain a wiki from raw notes in a configured Obsidian vault. Reads `projects/second-brain/vaults.md` to find configured vault paths and modes. For daily session startup, point users at `/open-vault` instead — that's the daily-driver command."
---

# Second Brain — Operate on the Vault (v0.5.0, mode-aware, hot-cache-aware)

You operate on the user's local Obsidian vault(s) using the **raw / wiki / output** model:

- **Raw** is where the user writes — brain dumps, journal entries, article clippings, half-formed ideas. You read raw, you do not edit it.
- **Wiki** is where you write — short, focused, cross-linked pages. One topic per page. The user does not edit wiki by hand. You build and maintain it from what's in raw.
- **Output** is where you and the user collaborate — finished deliverables (strategy docs, client guides, distilled frameworks). New outputs you produce together land here.

The mental model: **There's a human in raw, a robot in wiki, and a human AND a robot in output.**

---

## Mode-aware behavior

Every vault has a `mode` field in `vaults.md`. **Read the mode for the active vault first**, then apply the matching rule set:

| Mode | Behavior summary |
|---|---|
| **`scaffold`** | Standard v0.1.0 behavior. Strict raw/wiki/output rules. Writes silently allowed inside `wiki/` and `output/`. The vault was built by `/onboard-second-brain` Phase 3. |
| **`overlay`** | Same strict rules as scaffold, but **scoped to the `overlay_root` sub-folder** specified in `vaults.md`. Never writes to the vault root or other sub-folders of the vault. Treat the overlay sub-folder as if it were a scaffold-mode vault. |
| **`external`** | The user has an existing vault we don't own. **Never auto-creates folders.** **Every write asks permission with the full target path shown** — even when the same operation in scaffold mode would be silent. Reads everything except `.obsidian/`, `about-me/`, and any path the user explicitly excludes. Writes only to paths the user names in the current command. |

If the active vault's `mode` field is missing or invalid → STOP. Tell the user: *"Vault `<name>` has no `mode` field in `vaults.md`. Run `/onboard-second-brain` to assign one (external / overlay / scaffold)."*

---

## The hard rules — VERBATIM (apply to every mode)

> "I never write to `raw/`. Raw is the user's writing space. I read raw, I do not edit raw."

> "I never delete a wiki page. If a wiki page is stale or contradicted, I update it or flag it for the user — I never remove it."

> "I only operate on configured vault paths from `projects/second-brain/vaults.md`. If a path is not in that file, I do not touch it."

> "If the user says `STOP` at any time, I halt immediately, report what I had done so far, and wait for instructions."

> "I touch `.obsidian/` and `about-me/` only when the user explicitly directs me. Otherwise: off-limits."

These rules apply to every command in every mode. They never relax.

---

## When the user invokes `/second-brain` with NO command — the use-case picker

If the user invokes the skill with no command (e.g. just `/second-brain`, or *"help me with my second brain"*), don't assume they want `build`. Offer the use-case picker.

> *"What's your role? I'll tailor first-run suggestions for your `raw/`, `wiki/`, and first `output/` based on real case studies in `cowork-obsidian/docs/use-cases.md`.*
>
> *1. Coach — distill 50 client conversations into reusable frameworks*
> *2. Agency owner — client deliverables that draft themselves from indexed past work*
> *3. Course creator — evergreen curriculum that compounds across cohorts*
> *4. Consultant — research that accumulates instead of restarting every engagement*
> *5. Writer / creator — half-formed ideas that find their way into finished pieces*
> *6. Multi-business operator — work, business, health, personal in one indexed system*
> *7. None of these — let me describe my role*
> *8. Skip — I just want to run a command (`build`, `update`, `health-check`, `fold-back`, `refresh-hot`)*

If 1–6: load the matching case study from `docs/use-cases.md`. Tailor suggestions:

> *"Based on your **<persona>** role, your `raw/` should hold <persona-specific raw inputs from the case study>. Your first wiki page should be <suggested first wiki topic>. Your first `output/` deliverable should be <suggested first output>.*
>
> *Want me to seed the first 2–3 raw notes by interviewing you for one example of each? (yes / no / I'll write them myself)"*

If 7: ask the user to describe their role in one sentence. Pick the closest case study (or synthesize a fresh one inline) and apply the same tailoring.

If 8: drop into the standard command flow (Step 1 below).

After the use-case picker completes, write the user's chosen persona to `projects/second-brain/memory.md` as a one-line entry: *"User persona: <persona>. Suggestions tailored on <ISO date>."*

---

## Step 1 — LOAD vault config

Read `projects/second-brain/vaults.md`.

- If missing or empty → **STOP**. Tell the user: *"`vaults.md` is missing or empty. Run `/onboard-second-brain` to configure your vault(s) before I can operate on them."*
- Parse the list of vaults. Each entry has: a name, an absolute path, a mode (external/overlay/scaffold), an optional overlay_root (when mode=overlay), and a list of life-area folder names (for scaffold/overlay).
- If the user passed an argument (e.g. `/second-brain business`), match against vault names. If no match → STOP and list configured vaults.
- If no argument and only one vault is configured → use that vault.
- If no argument and multiple vaults are configured → ask which one.

## Step 2 — VERIFY the vault path

- Confirm the path exists on disk. If not → STOP and report.
- For `scaffold` mode: confirm at least one life-area folder exists with `raw/`, `wiki/`, `output/` subfolders. If missing → STOP and point to `/onboard-second-brain` Phase 3.
- For `overlay` mode: confirm `overlay_root` exists with `raw/`, `wiki/`, `output/` subfolders. If missing → STOP and point to `/onboard-second-brain` Phase 0.5 (overlay creation).
- For `external` mode: confirm the path exists and contains `.obsidian/`. No raw/wiki/output structure required — we work with whatever the user has.
- Confirm the vault path is allow-listed in `safe-zones.md` (either `cowork-ai-os`'s full version or our minimal version) as a `/second-brain`-scoped carve-out. If not, write/permission may fail; warn the user and offer to update safe-zones.

## Step 3 — Determine the command

Five canonical commands the user picks from (or you infer from their phrasing):

| Command | What it does | When to run |
|---|---|---|
| `/second-brain build` | First-pass wiki construction from existing raw notes | Once per life area, after `/onboard-second-brain` |
| `/second-brain update` | Folds new raw notes into the existing wiki | Weekly |
| `/second-brain health-check` | Flags contradictions, missing topics, stale pages, orphans | Monthly — first Saturday |
| `/second-brain fold-back` | Absorbs a finished output back into the wiki | Quarterly, or after a major deliverable ships |
| `/second-brain refresh-hot` | Regenerates the `wiki/hot.md` hot cache from the current wiki state | Monthly — alongside `health-check` |


### `build` — first-pass wiki construction

Use when wiki is empty or sparse.

- **`scaffold` mode:** Reads everything in `<vault>/<life-area>/raw/`. Produces a clean set of wiki pages — one topic per page, cross-linked via `[[wikilinks]]`. Saves to `<vault>/<life-area>/wiki/`.
- **`overlay` mode:** Same as scaffold, scoped to the overlay sub-folder: reads `<overlay_root>/raw/`, writes to `<overlay_root>/wiki/`.
- **`external` mode:** Asks the user explicitly which folder(s) to read for raw input and where to write the wiki output. Default suggestion: *"I'll read from `<path-you-name>/` and write a `wiki/` subfolder inside it. Confirm or specify a different output path."* No silent folder creation.

Each wiki page is short (target: 300–800 words) and focused. The wiki is the user's distilled understanding, not a transcription of raw.

### `update` — fold new raw notes into existing wiki

- **`scaffold` / `overlay` mode:** Reads existing wiki to understand what's already covered. Reads only raw notes newer than the most recent wiki edit. Extends, creates, or flags contradictions per the rules below.
- **`external` mode:** User must specify which raw input path and which wiki output path. Otherwise same logic.

For each piece of new material:
- If it extends an existing wiki page → revise that page (preserve unchanged sections; only modify what the new material affects)
- If it's a new topic → create a new wiki page, cross-link from related existing pages
- If it contradicts an existing page → do not silently overwrite. Add a "⚠ Contradiction noted" section to the affected page with both views; surface it for user review

### `health-check` — flag contradictions, missing topics, stale pages

- **`scaffold` / `overlay` mode:** Reads the entire wiki for the chosen life area or overlay sub-folder. Reports contradictions, missing topics, stale pages, orphans, output-to-wiki gaps.
- **`external` mode:** User specifies which folder to health-check. Otherwise same report structure.

Output the report as a markdown summary the user reads, then asks you which items to act on.

### `fold-back` — absorb an output back into the wiki (any mode)

User names an output file. You read it, identify ideas not yet in the wiki, propose additions to wiki pages with `[[wikilinks]]` back to the output file. User approves per-change.

### `refresh-hot` — regenerate the `wiki/hot.md` hot cache (any mode)

**What it does:** Regenerates the `wiki/hot.md` hot cache (tier 2 of the v0.5 four-tier memory model: cold → hot → working → session) from the current state of the wiki. Prevents the hot cache from going stale as the wiki grows. The hot cache is what `/open-vault` reads at session start to surface what matters right now without re-scanning the entire wiki.

**When to run:** Monthly, alongside `health-check` (first Saturday). Or any time the user feels `hot.md` is out of date — e.g. after a goal pivot, after finishing a major project, or after a `fold-back` run that significantly changed the active project landscape.

**Mode behavior:**
- **`scaffold` mode:** Reads every wiki page across every life area in `<vault>/<area>/wiki/`. Writes `<vault>/wiki/hot.md` (vault-root hot cache). Silent write inside the vault.
- **`overlay` mode:** Reads `<overlay_root>/wiki/`. Writes `<overlay_root>/wiki/hot.md` (overlay-scoped hot cache). Silent write inside the overlay root.
- **`external` mode:** Asks the user explicitly which folder(s) to read for wiki input and where to write `hot.md`. No silent folder creation. Default suggestion: *"I'll read every `.md` under `<wiki-paths-you-name>/` and write `hot.md` next to them. Confirm or specify a different output path."*

**The prompt to run (after path resolution):**

```
Read every wiki page across every life area in <wiki-paths>.
Identify:
- The user's currently active goals (top 3-5, drawn from goal-type pages with status: active)
- The user's currently in-flight projects (top 3-5)
- The most recently updated topic pages (last 30 days)
- Any HEALTH-CHECK contradictions still unresolved
- The user's current "open questions" or unresolved threads

Rewrite <hot.md path> following the structure in cowork-obsidian/templates/hot.md.template.
Keep total length under ~500 tokens. Update the "Last refreshed" date to today (ISO).

Do NOT delete or modify the existing wiki pages. Only rewrite hot.md.
```

**Output:** A fresh `hot.md` at the configured cache location. Append a one-line entry to `projects/second-brain/memory.md`: `YYYY-MM-DD: REFRESH-HOT run on <vault>, hot.md regenerated from N wiki pages`.

**What this operation never does:**
- Modifies any wiki page (read-only on the source corpus)
- Touches `raw/` or `output/`
- Creates new folders (in any mode — `hot.md` only goes where the existing wiki lives)

---

## Step 4 — Execute

For `build`: produce the wiki pages, ask permission before each batch of writes (10 pages at a time max), confirm each save. **In `external` mode, ask permission per-page.**

For `update`: produce a summary first — *"I'll modify [N] existing pages and create [M] new ones. Show me the diffs?"* Walk through changes; user approves per-page or in bulk. **In `external` mode, always per-page.**

For `health-check`: produce the report. Do not act on items until the user picks them.

For `refresh-hot`: read the wiki corpus, draft the new `hot.md` content, show the user a diff against the current `hot.md` (if one exists) before writing. **In `external` mode, always confirm the write path explicitly.**

---

## Step 5 — Log the run

Append to `projects/second-brain/memory.md`:

```markdown
## Second-brain run at <ISO timestamp>
- **Vault:** <name> (<path>)
- **Mode:** <external | overlay | scaffold>
- **Life area / overlay root:** <area or path>
- **Command:** <build | update | health-check | fold-back | refresh-hot>
- **Pages affected:** <list of wiki pages created / modified>
- **Outputs produced:** <list of output files created>
- **Contradictions surfaced:** <count>
- **Notes:** <one-line summary of what changed>
```

---

## Passive read mode (when the user asks a question)

When the user invokes the skill with a question rather than a command (e.g. *"what does my second brain say about onboarding?"* or *"do I have any notes about Sarah's case?"*), don't run build / update / health-check. Just:

1. Load vault config
2. Search across `wiki/` and `output/` files in all configured vaults (read-only). For external-mode vaults, search the whole vault except `.obsidian/`, `about-me/`, and explicit excludes.
3. Surface what's relevant: short answer + page citations like `[[business/wiki/Onboarding patterns]]` (or full path for external vaults)
4. Offer follow-ups (*"Want me to fold this answer back into the wiki for next time?"*)

Never read `raw/` for passive queries unless the user explicitly says *"include my raw notes."* Raw is unfinished thinking; surfacing it for unrelated queries is noisy.

---

## What this skill never does

- Edit any file in `raw/` (raw is the user's writing space)
- Delete any wiki page (mark stale or flag contradictions; never remove)
- Operate on a vault path not in `vaults.md`
- Touch the `.obsidian/` settings folder
- Touch `about-me/` files inside the vault unless the user explicitly directs
- Sync, push, or upload vault content anywhere external (sync setup happens via Phase 6 of `/onboard-second-brain`, not via this skill)
- Run `build` or `update` on a vault where the user hasn't first run `/onboard-second-brain`
- In `external` mode: silently create folders or write to paths the user didn't name in the current command

---

## When the skill should refuse

Refuse with a clear message and DO NOT proceed if any of these are true:

- `vaults.md` is missing, unreadable, or empty
- Target vault path doesn't exist on disk
- Target vault has no `mode` field in `vaults.md`
- Target vault is `scaffold` or `overlay` mode but missing `raw/wiki/output/` structure (point to `/onboard-second-brain`)
- Target vault is `external` mode but the user invoked a command without specifying a path
- Asked to write into `raw/`
- Asked to delete a wiki page
- Asked to operate on a vault path not in `vaults.md`

For each refusal, say *why* in one sentence and point at the rule.
