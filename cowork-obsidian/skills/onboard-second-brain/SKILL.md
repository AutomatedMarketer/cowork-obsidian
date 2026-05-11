---
name: onboard-second-brain
description: Walks the user through Project 04 — Second Brain in Obsidian. Triggers on "/onboard-second-brain", "start onboarding", "start second brain", "start the obsidian onboarding", "set up second brain", "set up obsidian vault", "install obsidian second brain", "build my second brain", "wire obsidian to cowork", "connect my existing obsidian vault", "begin second brain setup". v0.4.0 — Phase 4 now also recommends installing the official `obsidian:` skill pack (markdown / bases / json-canvas / obsidian-cli / defuddle) so Cowork authors current Markdown/Bases/Canvas syntax. v0.3.0 added `/open-vault` daily-driver. v0.2.0 added branching between connect-existing-vault and create-new-vault paths. Auto-detects existing Obsidian vaults. Three vault modes (external / overlay / scaffold). Soft cowork-ai-os prerequisite (works standalone if not present). 8 phases including optional sync setup and optional Claude Desktop MCP. Pause-friendly. Reads state from `_aibos/state-second-brain.md` and resumes at the correct phase. After onboarding, the user runs `/open-vault` daily and `/second-brain` for the operations.
---

# Onboard Second Brain — Install Skill (v0.4.0)

You are walking the user through installing the cowork-obsidian plugin — wiring a local Obsidian vault to Claude Cowork. By the end, the user has:

- An Obsidian vault wired to Cowork (connected from an existing vault, or scaffolded fresh)
- The vault path(s) registered in `projects/second-brain/vaults.md` with the right `mode` (`external` / `overlay` / `scaffold`)
- A safe-zones carve-out giving the `/second-brain` skill scoped read/write access to the vault
- The official `obsidian:` skill pack installed (or declined) — gives Cowork the current Markdown / Bases / Canvas syntax for vault authoring
- The three canonical prompts (`build`, `update`, `health-check`) tested and working
- Optional sync across machines (Obsidian Sync / Syncthing / Git+GitHub)
- Optional Claude Desktop MCP integration
- A weekly rhythm to keep the system alive

## Prerequisites — soft, not hard

This skill works standalone. It also integrates with `cowork-ai-os` if installed.

- **`cowork-ai-os` plugin v0.7.0+ (optional but recommended)** — provides Project 01 onboarding (`about-me/` files) and the full `safe-zones.md` model. If detected, we use its safe-zones. If not detected, we ship a self-contained minimal `safe-zones.md` and the user can still complete onboarding.
- **Project 01 (`/onboard`) completion** is now optional — only required if the user wants their `about-me/` files copied into the vault during scaffold. If the user has no `about-me/` folder, they can write fresh ones in Phase 3 (when scaffolding) or skip entirely (when connecting to an existing vault).
- **A local hard drive** for the vault folder location — cloud-sync of the vault folder is forbidden (corrupts writes). Sync across machines via Phase 6 options (Obsidian Sync / Syncthing / Git) is fine.

## The hard rule that governs everything

> **Local files. User owns the vault. No cloud upload of the vault folder. No external indexing.**

The vault lives on the user's disk. Cowork reads it. Obsidian edits it. Optional Phase 6 sync uses application-layer or version-control approaches — never cloud-sync of the folder bytes.

This skill never installs the Local REST API community plugin (we don't need it for Cowork), and only sets up an MCP server in Phase 7 if the user explicitly wants Claude Desktop integration.

If the user asks to bypass the local-only model for the vault folder ("auto-cloud-sync the folder," etc.), refuse and explain the corruption risk.

---

## How to run this skill

### Step 1 — Read the state file

Read `_aibos/state-second-brain.md`. Four cases:

- **Doesn't exist** → User starting fresh. Create it from `templates/state-second-brain.template.md`. Run Phase 0 (Welcome).
- **Exists, `current_phase: <N>`** → User resuming. Load `phases/0N-*.md` (or `phases/00.5-connect-or-create.md` if `current_phase: 0.5`) and run that phase. **Before running**: if `obsidian_companion_skills` field is missing (v0.2.0/v0.3.0 state), append `obsidian_companion_skills: not_checked` and `companion skills installed (obsidian:*): false` to the state file silently — Phase 4 Step 5 will fill them in when reached.
- **Exists, `vault_mode_default` is missing (v0.1.0 state)** → Migrate. Tell the user *"I see a v0.1.0 state file. I'll add the new v0.2.0 fields and assume `vault_mode = scaffold` for existing vaults (the v0.1.0 default). Confirm?"* On yes: append `vault_mode_default: scaffold`, `cowork_ai_os: <detected|not_detected>`, `phase_6_sync_choice: skipped`, `phase_7_desktop_mcp: skipped`, `obsidian_companion_skills: not_checked`, then resume at the user's `current_phase`.
- **Exists, `second_brain_complete: true`** → User finished initial install. Confirm completion. **If `obsidian_companion_skills` field is missing OR set to `not_checked`** (e.g., user finished onboarding on v0.3.0 or earlier), proactively offer: *"You're on a v0.3.0-era install — there's a new companion skill pack (v0.4.0) that gives Cowork current Obsidian file-format syntax. Want me to run Phase 4 Step 5 now to detect/install it? (~1 minute.)"* On yes: jump to Phase 4 Step 5, run it, then return to the completion menu. On no: append `obsidian_companion_skills: declined` and continue. Then the standard completion menu: add another vault, run health-check, walk a wiki page, set up sync (Phase 6 if skipped), set up Desktop MCP (Phase 7 if skipped), install companion skill pack (Step 5 if `obsidian_companion_skills` is not `installed` / `already_installed`).

### Step 2 — Soft prerequisite check

Before Phase 0, run these checks. **None are STOPs by themselves.** Each surfaces an option the user picks:

1. `_aibos/state.md` exists with `core_setup_complete: true`?
   - **Yes** → cohort-aligned path. Use existing about-me files later.
   - **No** → still fine. Tell user: *"You don't have Project 01 (Core Setup) complete. That's optional for cowork-obsidian. Two options: (a) install cowork-ai-os and finish Project 01 first (recommended for cohort users), or (b) continue here — we'll write fresh about-me files in Phase 3 if needed."* Capture `cowork_ai_os_p01_complete: false`.

2. `cowork-ai-os` plugin folder exists at v0.7.0+?
   - **Yes** → integrate with its `safe-zones.md`. Capture `cowork_ai_os: detected`.
   - **No** → still fine. Tell user: *"cowork-ai-os not detected. We'll ship a self-contained minimal safe-zones in Phase 4. Optional — install cowork-ai-os later if you want morning brief, file tidy, etc."* Capture `cowork_ai_os: not_detected`.

3. `projects/file-organization/safe-zones.md` exists?
   - **Yes** → we'll add our carve-out in Phase 4.
   - **No** → we'll create a minimal one in Phase 4. Capture `safe_zones_path: minimal_in_phase_4`.

### Step 3 — Run the phase script

Each phase script in `phases/0N-*.md` (or `phases/00.5-connect-or-create.md`) is the source of truth. Follow it exactly. Pause after each phase for user confirmation before advancing.

### Step 4 — Sample-first option

At Phase 1 (planning vaults — only entered if user chose path B in Phase 0.5), offer to show the user a sample `vaults.md` first. Read from `samples/sample-vaults.md`.

### Step 5 — Pause and resume

If the user types `pause onboarding`: save state. Tell them: *"Phase X complete. You're [N]% through. Resume any time with `/onboard-second-brain`."*

If user types `continue onboarding` or invokes the skill again: read state, jump to `current_phase`.

### Step 6 — Phase completion

After each phase's verification passes:
1. Update state file: `current_phase: <next>`, append timestamped log entry
2. Tell user: *"Phase N complete. You're [%] through. [One-sentence preview of next phase]. Type `continue onboarding` when ready."*

### Step 7 — Final phase

After the last phase the user runs (Phase 5 if they skip 6 and 7, or 6, or 7) verification passes:
- Set `second_brain_complete: true`
- Set `completed_at: <ISO timestamp>`
- Tell the user: *"cowork-obsidian install complete. Your vault is wired. Run `/second-brain build [life-area]` any time to construct or update a wiki. Come back next week to test the rhythm."*

---

## The 8 phases at a glance

| Phase | What it does | Time | Required? |
|---|---|---|---|
| 0 — Welcome | Soft prereq check, scaffold `projects/second-brain/`, explain raw/wiki/output | ~5 min | Yes |
| 0.5 — Connect or create | **NEW.** Auto-detect existing Obsidian vaults from `obsidian.json`. Connect to one (mode `external` or `overlay`) OR fall through to Phase 1 to scaffold | ~5–10 min | Yes |
| 1 — Plan vaults | Interview to capture `vaults.md` config | ~10 min | Only if creating new |
| 2 — Install Obsidian | Auto-detect + walk download | ~5 min | Only if creating new |
| 3 — Scaffold first vault | Pick local-drive path (forbid cloud-sync), create life-area structure, copy/write `about-me/` | ~15 min | Only if creating new |
| 4 — Wire Cowork to vault | Add vault path to `safe-zones.md` as `/second-brain`-scoped carve-out, test read/write | ~5 min | Yes |
| 5 — The three prompts | Run `build` for the first time. Mode-aware (external/overlay/scaffold). Establish weekly rhythm | ~30 min | Yes |
| 6 — Sync setup | **NEW.** Optional. Decision-tree across Obsidian Sync, Syncthing, Git/GitHub | ~10–15 min | Optional |
| 7 — Claude Desktop MCP | **NEW.** Optional. Wire a separate Claude Desktop chat app to the same vault, with the GoHighLevel Mac trap warnings | ~10 min | Optional |

**Branch logic:** Phase 0 → Phase 0.5 → (path A: skip 1/2/3, jump to 4) OR (path B: continue 1 → 2 → 3 → 4) → 5 → optional 6 → optional 7 → done.

---

## Cross-skill coordination

- **`cowork-ai-os` integration** is opt-in based on detection. If installed at v0.7.0+, we use its `safe-zones.md`. If not, we ship our own minimal version.
- The `vaults.md` config we create here will be **read by future cowork-ai-os v0.8.0+** so `/morning-brief` and `/voice-writer` can optionally pull from the vault. That cross-skill wiring is deferred.
- The `/second-brain` operational skill is mode-aware (reads `mode` field from `vaults.md`). It's the only skill that should write into vault folders. `/tidy-downloads` (from cowork-ai-os) will not touch the vault path because of the carve-out's scoping.
- **The official `obsidian:` skill pack** (5 skills: `obsidian-markdown`, `obsidian-bases`, `json-canvas`, `obsidian-cli`, `defuddle`) is the recommended companion. Phase 4 Step 5 detects it and offers install if missing. cowork-obsidian provides the *workflow* layer; the official pack provides the *file-format intelligence*. When `/second-brain` authors a `.md` / `.base` / `.canvas` file in the vault, it should invoke the matching `obsidian:*` skill first so the syntax is current (Bases shipped in 2025 and keeps evolving — training-data knowledge of the format can be stale).

---

## What this skill never does

- Force the raw/wiki/output overlay onto a user's existing vault structure (use `external` mode if connecting to an existing vault — no overlay imposed)
- Install the Obsidian Local REST API community plugin (not needed for Cowork)
- Set up an MCP server unless the user explicitly opts into Phase 7 (Cowork doesn't need MCP — it has filesystem access)
- Hard-stop on missing `cowork-ai-os` (soft prereq — provide minimal alternatives)
- Allow the vault folder location to be inside iCloud / OneDrive / Dropbox / Google Drive auto-sync folders (Phase 3 forbids these for new vaults; Phase 0.5 warns existing-vault users if their vault is in such a path)
- Bulk-import the user's existing note app history (push back — pull specific valuable pieces, not whole dumps)
- Allow more than 4 life areas per scaffold-mode vault (push back at Phase 1 — macro beats micro)
- Sync the vault folder via cloud-storage services (Phase 6 uses application-layer Obsidian Sync, peer-to-peer Syncthing, or version-control Git — never folder-byte sync)

If asked to do any of the above, refuse and explain which rule it violates.
