# Phase 0 — Welcome

**Goal:** Greet, set expectations, verify prerequisites (cowork-aibos v0.7.0+, P01 complete), scaffold `projects/second-brain/`, create the state file.

**Time:** ~5 minutes for Phase 0. ~10 minutes for Phase 1 (next).

---

## Script — what to say (in voice)

When the user runs `/onboard-second-brain` for the first time:

> *"Welcome to Project 04 — Second Brain in Obsidian.*
>
> *By the end of this install you'll have a permanent local-first knowledge base on your computer that grows for years. You'll write into a `raw/` folder. I'll build and maintain a clean wiki from what's in raw. Deliverables you and I produce together land in `output/`. Every note is a plain markdown file — you own it forever. No cloud lock-in.*
>
> *Six phases, about 70 minutes total. Pause-friendly.*
>
> *What this is NOT: I will not install the Obsidian Local REST API plugin, I will not set up an MCP server, I will not ask you to use VS Code or git. The vault works as plain files. Cowork reads them directly. Simpler is the point.*
>
> *Want to see a sample `vaults.md` config first? It's the file we'll build in Phase 1 — seeing one filled in makes the questions land faster. Type `show me a sample` for sample-first, or `let's go` to start."*

---

## If user says `show me a sample`

1. Read `samples/sample-vaults.md` from this skill folder
2. Display it in full
3. Then say:

> *"That's an example with three vaults — one per context. Yours might just be one for now. We'll figure that out in Phase 1. Type `let's go` when ready."*

## If user says `let's go`

Run the verification + scaffolding sequence below.

---

## Prerequisite verification

Execute these checks in order. STOP at the first failure.

### Check 1 — Project 01 complete

Read `_aibos/state.md`. Confirm `core_setup_complete: true`.

If false:

> *"Project 01 (Core Setup) isn't complete. The second brain pulls from your `about-me/` files (About Me, Brand Voice, AI Working Style) — those need to exist first. Run `/onboard` (from the cowork-aibos plugin) and finish Project 01. Come back here after."*

STOP.

### Check 2 — cowork-aibos plugin v0.7.0+

Look for `cowork-aibos/.claude-plugin/plugin.json` in the plugin install. Read the version. Must be 0.7.0 or later.

If missing or older:

> *"This plugin (cowork-second-brain) depends on cowork-aibos v0.7.0+ for the safe-zones model that protects your filesystem. Yours is [version-found / not-installed]."*
>
> **Mac:** *"Download the latest cowork-aibos.zip from https://github.com/AutomatedMarketer/cowork-aibos/releases/latest, then in Claude Desktop go to Settings → Customize → Browse plugins → upload. Once installed/updated, run `/onboard-second-brain` again."*
>
> **Windows:** *"Run `/plugin install cowork-aibos@cowork-aibos` (or `/plugin update cowork-aibos` if already installed), then run `/onboard-second-brain` again."*

STOP.

### Check 3 — safe-zones.md exists OR willingness to create

Look for `projects/file-organization/safe-zones.md`. If it exists, great — we'll add our carve-out in Phase 4.

If it doesn't exist (the user hasn't run `/onboard-file-organization` from cowork-aibos):

> *"You don't have a `safe-zones.md` file yet — that's normally created when you run `/onboard-file-organization` (from cowork-aibos). Two options:*
>
> *1. Run `/onboard-file-organization` first, then come back. Recommended if you also want the file-tidy feature. ~90 minutes.*
> *2. Skip the full file-organization onboarding and just create a minimal `safe-zones.md` here that ONLY allows the second-brain skill to write to your vault. Faster (~2 minutes) but you don't get `/tidy-downloads`. Recommended if you only want the second brain right now.*
>
> *Which one?"*

If option 1: STOP and tell the user to come back after `/onboard-file-organization`.
If option 2: capture the choice; in Phase 4 we'll create a minimal `safe-zones.md` with just the second-brain carve-out.

---

## Scaffolding sequence

### Step 1 — Create projects/second-brain/

Create (if missing):

- `projects/second-brain/` — the project folder
- `projects/second-brain/memory.md` — empty audit log (use `templates/memory.template.md` if present in this skill folder, otherwise create with header `# Second Brain — Memory & Audit Log` + first dated entry)

Ask permission before each write.

### Step 2 — Create state file

Write `_aibos/state-second-brain.md` from `templates/state-second-brain.template.md`. Set:

- `started_at: <ISO timestamp>`
- `current_phase: 1`
- `Phase 0 (welcome): completed at <ISO timestamp>`

Capture from Check 3 whether the user picked Option 1 (existing safe-zones.md) or Option 2 (minimal safe-zones in Phase 4) — store as `safe_zones_path: existing` or `safe_zones_path: minimal_in_phase_4`.

### Step 3 — Confirm and preview Phase 1

Tell the user:

> *"Phase 0 done. Project folder created. State saved.*
>
> *Phase 1 next: I'll interview you about your vault setup. Most beginners want one vault. Power users like Nuno run several — one for business stuff, one for personal, one for a specific project. We'll figure out yours and capture it in a config file called `vaults.md`. About 10 minutes.*
>
> *Then Phase 2: Obsidian install. About 5 minutes — you download it, I help you launch and pick a vault location.*
>
> *Type `continue onboarding` when ready."*

---

## Verification before advancing

Phase 0 is complete when ALL of these are true:

- `_aibos/state.md` confirms `core_setup_complete: true`
- `cowork-aibos` plugin is at 0.7.0 or later
- The user has chosen how to handle safe-zones (existing or minimal-in-Phase-4)
- `projects/second-brain/` folder exists
- `projects/second-brain/memory.md` exists with the start entry
- `_aibos/state-second-brain.md` exists with `current_phase: 1`

If any check fails, STOP and resolve before advancing.
