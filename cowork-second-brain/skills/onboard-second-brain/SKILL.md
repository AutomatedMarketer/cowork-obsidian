---
name: onboard-second-brain
description: Walks the user through Project 04 — Second Brain in Obsidian. Triggers on "/onboard-second-brain", "set up second brain", "set up obsidian vault", "install obsidian second brain", "build my second brain", "wire obsidian to cowork". Six phases, ~70 minutes total. Pause-friendly. Builds a local-first Obsidian vault (multi-vault aware), wires it to Claude Cowork via a safe-zones carve-out, and runs the three canonical prompts (build / update / health-check) for the first time. No MCP, no API keys, no Local REST API plugin. No VS Code, no git. Reads state from `_aibos/state-second-brain.md` and resumes at the correct phase.
---

# Onboard Second Brain — Project 04 Install Skill

You are walking the user through Project 04 — Second Brain in Obsidian. By the end, the user has:

- Obsidian installed
- One or more local vaults scaffolded with the raw / wiki / output structure
- The vault path(s) wired to Cowork via a `/second-brain`-scoped carve-out in `cowork-aibos`'s `safe-zones.md`
- The three canonical prompts (`build`, `update`, `health-check`) tested and working
- A weekly rhythm to keep it alive

## Prerequisites

This skill expects:

- **`cowork-aibos` plugin v0.7.0+** installed (provides Project 01 onboarding and the `safe-zones.md` mechanism for cross-skill coordination)
- **Project 01 (`/onboard`) complete** — workspace, about-me files, connectors all set up
- A local hard drive (NOT a cloud-sync folder) for the vault

If any prerequisite is missing, Phase 0 catches it and tells the user how to fix.

## The hard rule that governs everything

> **Local files. User owns the vault. No cloud upload, no external indexing.**

The vault lives on the user's disk. Cowork reads it. Obsidian edits it. Nothing else.

This skill never enables Obsidian Sync without explicit user request, never installs the Local REST API community plugin (we don't need it), never sets up an MCP server (we don't need that either for v0.1.0). The vault is plain markdown files; Cowork reads and writes them directly via filesystem access, scoped by the safe-zones carve-out.

If the user asks to bypass the local-only model for "convenience" (auto-cloud-sync, auto-upload, etc.), refuse and explain why portability and ownership matter.

---

## How to run this skill

### Step 1 — Read the state file

Read `_aibos/state-second-brain.md`. Three cases:

- **Doesn't exist** → User starting fresh. Create it from `templates/state-second-brain.template.md`. Run Phase 0 (Welcome).
- **Exists, `current_phase: <N>`** → User resuming. Load `phases/0N-*.md` and run that phase.
- **Exists, `second_brain_complete: true`** → User finished initial install. Confirm completion. Offer: add another vault, run health-check, or walk a wiki page.

### Step 2 — Verify prerequisites

Before Phase 0, confirm:

1. `_aibos/state.md` exists and shows `core_setup_complete: true` (Project 01 done)
2. `cowork-aibos` plugin folder exists with version 0.7.0 or later (check `cowork-aibos/.claude-plugin/plugin.json`)
3. `cowork-aibos` has the `safe-zones.md` mechanism — i.e., `projects/file-organization/safe-zones.md` exists, OR the user is willing to create a basic safe-zones file in Phase 4

If any check fails, Phase 0 surfaces the gap and gives a fix path.

### Step 3 — Run the phase script

Each phase script in `phases/0N-*.md` is the source of truth. Follow it exactly. Pause after each phase for user confirmation before advancing.

### Step 4 — Sample-first option

At Phase 1 (planning vaults), offer to show the user a sample `vaults.md` first. Read from `samples/sample-vaults.md`.

### Step 5 — Pause and resume

If the user types `pause onboarding`: save state. Tell them: *"Phase X complete. You're [N]% through Project 04. Resume any time with `/onboard-second-brain`."*

If user types `continue onboarding` or invokes the skill again: read state, jump to `current_phase`.

### Step 6 — Phase completion

After each phase's verification passes:
1. Update state file: `current_phase: N+1`, append timestamped log entry
2. Tell user: *"Phase N complete. You're [%] through. [One-sentence preview of next phase]. Type `continue onboarding` when ready."*

### Step 7 — Final phase

After Phase 5 verification passes:
- Set `second_brain_complete: true`
- Set `completed_at: <ISO timestamp>`
- Tell the user: *"Project 04 install complete. Your vault is wired. Run `/second-brain build [life-area]` any time to construct or update a wiki. Come back next week to test the rhythm."*

---

## The six phases at a glance

| Phase | What it does | Time |
| --- | --- | --- |
| 0 — Welcome | Verify cowork-aibos v0.7.0+, P01 complete, scaffold `projects/second-brain/`, explain the raw/wiki/output model | ~5 min |
| 1 — Plan vaults | Interview: 1 vault or many? What life areas? Capture `vaults.md` config | ~10 min |
| 2 — Install Obsidian | Walk through obsidian.md download, first launch, "Create new vault" dialog. Detect existing install and skip. | ~5 min |
| 3 — Scaffold first vault | Pick local-drive path (forbid cloud-sync), create life-area structure (raw/wiki/output × N), copy `about-me/` from workspace into vault | ~15 min |
| 4 — Wire Cowork to vault | Add vault path to `safe-zones.md` as `/second-brain`-scoped carve-out, test by reading a note. No MCP, no API keys. | ~5 min |
| 5 — The three prompts | Run `build` / `update` / `health-check` for the first time on a seeded life area. Establish weekly rhythm. | ~30 min |

---

## Cross-skill coordination

- This plugin **depends** on `cowork-aibos` v0.7.0+ (uses the `safe-zones.md` mechanism for the carve-out).
- The `vaults.md` config we create here will be **read by future cowork-aibos v0.8.0+** so `/morning-brief` and `/voice-writer` can optionally pull from the vault. That cross-skill wiring is deferred — Project 04 v0.1.0 ships standalone.
- The `/second-brain` operational skill we configure here is the only skill that should write into `wiki/` and `output/`. `/tidy-downloads` will not touch the vault path because of the carve-out's scoping.

---

## What this skill never does

- Install the Obsidian Local REST API community plugin (we don't need it)
- Set up an MCP server (we don't need it for v0.1.0)
- Tell the user to install VS Code or git (cohort path is Cowork-only)
- Allow the vault to be created inside iCloud / OneDrive / Dropbox / Google Drive auto-sync folders (Phase 3 forbids these)
- Bulk-import the user's existing note app history (push back — pull specific valuable pieces, not whole dumps)
- Allow more than 4 life areas per vault (push back at Phase 1 — macro beats micro)

If asked to do any of the above, refuse and explain which rule it violates.
