# Phase 5 — The Three Canonical Prompts

**Goal:** Run `build` for the first time on the seeded life area, explain `update` and `health-check` for future use, establish a weekly rhythm, and write the first wiki pages. By the end, the vault has real content the user can read in Obsidian.

**Time:** ~30 minutes.

---

## Script — what to say (in voice)

> *"Phase 5 is the payoff. Three prompts run your second brain forever. You'll write the first one once and re-use them for years.*
>
> *- `build` — first-pass wiki construction from raw notes. Run when wiki/ is empty or sparse.*
> *- `update` — fold new raw notes into the existing wiki. Run weekly (or whenever you've added new raw material).*
> *- `health-check` — flag contradictions, missing topics, stale pages. Run monthly.*
>
> *Today we run `build` against your seeded raw notes. By the end you'll have your first wiki pages. We'll cover `update` and `health-check` conceptually — you'll run those for real next week.*
>
> *Type `let's build` when ready."*

---

## Step 1 — Run `build` for the first time

Pick the life area where Phase 3 seeded raw notes. Confirm with the user:

> *"Phase 3 seeded raw notes in your [life area] folder. We'll run `build` against that life area now. The skill will:*
>
> *1. Read every file in `<vault>/<life-area>/raw/`*
> *2. Read your `about-me/` files (About Me, Brand Voice, AI Working Style) for context and tone*
> *3. Construct a small set of wiki pages — one topic per page, cross-linked*
> *4. Show you the drafts before saving*
>
> *Sound good?"*

Invoke the operational skill: `/second-brain build [life-area]`.

The skill should:
1. Load `vaults.md` → find the configured vault and life area
2. Verify safe-zones permission for the vault path
3. Read raw notes
4. Read `about-me/` files
5. Synthesize topics — one wiki page per topic, with `[[wikilinks]]` between related pages
6. Present drafts to the user for review

User reviews drafts. Edits if needed. Approves saves. Each wiki page lands in `<vault>/<life-area>/wiki/`.

After saves complete, tell the user:

> *"Open Obsidian. Navigate to `<life area>/wiki/`. You should see [N] new pages. Click one — note that the cross-references in the text show up as clickable wikilinks in Obsidian. That's how the wiki becomes navigable as a graph, not just a list of files."*

---

## Step 2 — Explain `update` (don't run it yet)

> *"Next week, when you've added more raw notes (you write into `raw/` whenever inspiration strikes — could be daily, could be a few times a week), you'll run `update` instead of `build`. Same shape, but `update`:*
>
> *- Reads your existing wiki to understand what's already covered*
> *- Reads only the raw notes added since the last update*
> *- For each new note: extends an existing wiki page, OR creates a new one, OR flags a contradiction with what you already had*
>
> *We'll wait until you have at least 2–3 new raw notes before running `update` for the first time. Probably next week."*

---

## Step 3 — Explain `health-check` (don't run it yet)

> *"Once a month, you'll run `health-check`. It reads the entire wiki for one life area and reports:*
>
> *- **Contradictions** — places where two wiki pages disagree*
> *- **Missing topics** — topics in raw that never made it to wiki*
> *- **Stale pages** — wiki pages with no recent raw activity*
> *- **Orphans** — wiki pages with no cross-links*
>
> *You read the report. You pick which items to act on. The skill never silently 'fixes' things — it surfaces, you decide."*

---

## Step 4 — Establish weekly rhythm

Ask the user:

> *"Last setup question: when do you want to run `update` weekly? Some options:*
>
> *- Sunday evening as part of your week-prep ritual*
> *- Friday afternoon after the work week ends*
> *- Monday morning with coffee, reviewing what your past-week-self wrote*
>
> *Or you can run it ad-hoc whenever you've added enough raw notes. No schedule needed if you don't want one."*

Capture the user's preference. Optionally, schedule a recurring task (similar to how cowork-aibos schedules the morning brief and weekly tidy):

> *"Want me to schedule a weekly reminder that fires [day] [time]? It writes a 'TIME TO UPDATE' entry in `projects/second-brain/memory.md`, optionally drafts an email reminder to yourself in Gmail Drafts. The reminder is just a reminder — it doesn't run `update` automatically. You decide when to actually run it."*

If yes, schedule the reminder. If no, skip.

---

## Step 5 — Write `My Vault System.md` (deferred from Phase 3)

> *"Last file. `<vault>/about-me/My Vault System.md` documents how YOU actually use the vault — what kinds of things go in raw, how you decide which life area something belongs to, what `output/` means in your workflow. It's part of about-me/ so I read it on every session.*
>
> *We deferred writing it in Phase 3 because you hadn't used the vault yet. Now you've run `build` once. Want to draft `My Vault System.md` now (~5 minutes), or wait two weeks until you've used it more?"*

If now: short interview, save the file in `<vault>/about-me/My Vault System.md`.
If later: capture in state file as `my_vault_system_due: <date 2 weeks from now>` and remind on next skill invocation.

---

## Step 6 — Final wrap-up

Tell the user:

> *"Project 04 install complete.*
>
> *What you have now:*
> *- A local vault at [path] with [N] life areas, each with raw/wiki/output*
> *- About-me identity files copied / written into the vault*
> *- First wiki pages built from your seeded raw notes — open Obsidian and read them*
> *- Cowork wired to the vault via the safe-zones carve-out*
> *- The three prompts (`build` / `update` / `health-check`) tested and working*
> *- Optional weekly reminder for `update`*
>
> *What you do from here:*
> *1. Write into raw/ as ideas come up. Just plain markdown. Date prefix the filenames so they sort.*
> *2. Run `/second-brain update [life-area]` weekly (or when you've accumulated 2–3 new raw notes).*
> *3. Run `/second-brain health-check [life-area]` monthly.*
> *4. Save anything great that came out of a Cowork session into `<life-area>/output/`.*
> *5. Optionally tell me to fold an output back into the wiki: `/second-brain fold-back [output filename]`.*
>
> *The system compounds. After 6 months your wiki is rich enough that any new Cowork session can pull deep context without you uploading anything. After 2 years, it's a real second brain.*
>
> *Want to add another vault now (you have [N] more configured) or wrap up here?"*

If add another: loop back to Phase 3 with the next vault from `vaults.md`. (Phases 4 and 5 will need to run again for that vault too — but Phase 4 is just adding another safe-zones entry, fast.)

If wrap up: complete the install.

---

## Update state — install complete

Update `_aibos/state-second-brain.md`:
- `current_phase: 6` (sentinel — past Phase 5)
- `first build run: true`
- `weekly rhythm established: true`
- `second_brain_complete: true`
- `completed_at: <ISO timestamp>`
- `Phase 5 (three prompts): completed at <ISO timestamp>`

Append a final entry to `projects/second-brain/memory.md`:

```markdown
## <ISO timestamp> — Project 04 install complete

- Vaults configured: <count> (<list of names>)
- First vault path: <path>
- Life areas total: <count>
- First build ran on: <vault/life-area>
- Wiki pages created: <count>
- Weekly update reminder: <day/time, or "none">
- My Vault System.md: <written / deferred>
```

---

## Verification before calling done

- At least one life area has wiki pages in `wiki/`
- The user has opened Obsidian and confirmed seeing the wiki pages
- `vaults.md` reflects the actual vault on disk
- The safe-zones carve-out is in place (set in Phase 4)
- `_aibos/state-second-brain.md` shows `second_brain_complete: true`

---

## What this phase never does

- Run `build` on a life area without raw notes seeded (push back — set up raw notes first)
- Auto-schedule `update` to run AND auto-execute (auto-run is fine; auto-execute is not — `update` always shows the user the diffs before saving)
- Bulk-import existing notes from another app (push back — pull specific valuable pieces, not whole dumps)
- Write to `raw/` (raw is the user's writing space, ALWAYS)
