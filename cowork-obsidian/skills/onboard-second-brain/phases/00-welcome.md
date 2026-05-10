# Phase 0 — Welcome

**Goal:** Greet, set expectations, run **soft** prerequisite checks (cowork-ai-os is now optional), scaffold `projects/second-brain/`, create the state file. v0.2.0 also previews Phase 0.5 — the new branch where the user picks "connect existing vault" or "create new vault."

**Time:** ~5 minutes for Phase 0. ~5–10 minutes for Phase 0.5 (next).

---

## Script — what to say (in voice)

When the user runs `/onboard-second-brain` for the first time:

> *"Welcome to cowork-obsidian — your second brain in Obsidian, wired to Claude Cowork.*
>
> *By the end of this install you'll have a local-first knowledge base on your computer that grows for years. You'll write into a `raw/` folder. I'll build and maintain a clean wiki from what's in raw. Deliverables you and I produce together land in `output/`. Every note is a plain markdown file — you own it forever. No cloud lock-in.*
>
> *Up to 8 phases, ~70 minutes if you're scaffolding a fresh vault, much less if you're connecting to an existing one. Pause-friendly.*
>
> *What this is NOT: for the main flow, I will not install the Obsidian Local REST API plugin, I will not set up an MCP server (Cowork has filesystem access already), I will not ask you to use VS Code or git. The vault works as plain files. Cowork reads them directly. Simpler is the point. (MCP becomes optional in Phase 7 if you also use Claude Desktop.)*
>
> *Next up: Phase 0.5 — I'll auto-detect any Obsidian vaults you already have on this machine. If you've got one, you can connect to it (no reorganizing your structure). If you don't, we scaffold a fresh one in Phases 1–3.*
>
> *Want to see a sample `vaults.md` config first? Seeing one filled in makes the planning questions land faster. Type `show me a sample` for sample-first, or `let's go` to start."*

---

## If user says `show me a sample`

1. Read `samples/sample-vaults.md` from this skill folder
2. Display it in full
3. Then say:

> *"That's an example with three vaults — one per context. Yours might just be one for now. We'll figure that out in Phase 1. Type `let's go` when ready."*

## If user says `let's go`

Run the verification + scaffolding sequence below.

---

## Soft prerequisite check (v0.2.0)

Execute these checks in order. **None are STOPs by themselves** — each surfaces an option.

### Check 1 — Project 01 complete (optional)

Read `_aibos/state.md`. Confirm `core_setup_complete: true` if the file exists.

- **If true** → cohort-aligned path. Use existing about-me files later.
- **If false (or `_aibos/state.md` missing)** → tell user:
  > *"Optional: Project 01 (Core Setup) from cowork-ai-os builds your `about-me/` files (About Me, Brand Voice, AI Working Style). Two paths:*
  >
  > *(a) Install cowork-ai-os and run `/onboard` first — recommended for VCInc cohort users.*
  > *(b) Continue here. We'll write fresh about-me files in Phase 3 if needed (or skip them entirely if you're connecting to an existing vault that already has identity files)."*

  Capture `cowork_ai_os_p01_complete: false`. Continue.

### Check 2 — cowork-ai-os plugin (optional)

Look for `cowork-ai-os/.claude-plugin/plugin.json` in the plugin install. Try to read the `version` field.

- **If found at v0.7.0+** → integrate. Capture `cowork_ai_os: detected`.
- **If found but older** → tell user *"You have cowork-ai-os v<X>. cowork-obsidian benefits from v0.7.0+ but works without it. Continuing standalone."* Capture `cowork_ai_os: detected_old`.
- **If missing** → tell user:
  > *"cowork-ai-os not detected. cowork-obsidian works fine standalone — we'll ship a self-contained minimal `safe-zones.md` in Phase 4. Optional: install cowork-ai-os later if you want `/morning-brief`, `/tidy-downloads`, etc."*

  Capture `cowork_ai_os: not_detected`. Continue.

### Check 3 — safe-zones.md location

- **If `projects/file-organization/safe-zones.md` exists** (cowork-ai-os installed and `/onboard-file-organization` was run) → we'll add our carve-out to it in Phase 4. Capture `safe_zones_path: existing`.
- **If missing** → we'll create a minimal version in Phase 4. Capture `safe_zones_path: minimal_in_phase_4`. No user prompt needed for this — minimal is fine for cowork-obsidian alone.

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
- `current_phase: 0.5`
- `Phase 0 (welcome): completed at <ISO timestamp>`
- `cowork_ai_os: <detected | detected_old | not_detected>` (from Check 2)
- `cowork_ai_os_p01_complete: <true | false>` (from Check 1)
- `safe_zones_path: <existing | minimal_in_phase_4>` (from Check 3)

### Step 3 — Confirm and preview Phase 0.5

Tell the user:

> *"Phase 0 done. Project folder created. State saved.*
>
> *Phase 0.5 next — the fork in the road. I'll auto-detect any Obsidian vaults you already have on this machine by parsing Obsidian's own config file. Two outcomes:*
>
> *(A) I find one or more vaults → you pick one to connect to (we don't reorganize your structure), OR you tell me to create a fresh one.*
> *(B) I find none (or auto-detect fails) → we move to Phase 1 to plan and scaffold a new vault.*
>
> *Either way, this should take 5–10 minutes. Type `continue onboarding` when ready."*

---

## Verification before advancing

Phase 0 is complete when ALL of these are true:

- The 3 soft prereq checks ran (no STOPs anymore — each one captured a choice)
- `projects/second-brain/` folder exists
- `projects/second-brain/memory.md` exists with the start entry
- `_aibos/state-second-brain.md` exists with `current_phase: 0.5` and the `cowork_ai_os`, `cowork_ai_os_p01_complete`, `safe_zones_path` fields populated

If any of these fail, surface the error and let the user retry. Do not STOP unless something fundamental is broken (e.g., can't write to disk).
