# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.3.0] — 2026-05-09

### Added

- **New skill: `/open-vault`** — daily-driver session starter. Launches Obsidian to the configured vault via the `obsidian://open?vault=<name>` URL scheme AND preps the Cowork session: loads `vaults.md`, reports vault status (mode, last activity, new raw notes since last update), offers a next-step menu (build / update / health-check / fold-back / browse). The "every time you sit down to work on your second brain" command. Triggers on `/open-vault`, `open my vault`, `let me work on my second brain`, `start second brain session`.
- **Natural-language triggers** added across all 3 skills so beginners don't need to memorize slash commands. `/onboard-second-brain` now also fires on "start onboarding"/"start second brain"/"begin second brain setup". `/second-brain` adds "refresh my wiki", "audit my second brain", "run my second brain", "process my raw notes".
- **Lesson 5 SOP — Use Cases & Daily Workflow** at `cowork-obsidian/docs/sops/SOP-C04-05.md`. Read-only "now what?" guide: 6 case studies by role (coach / agency / course creator / consultant / writer / multi-business operator), references to second-brain thinkers (Julie Chenell, Tiago Forte / PARA, Andy Matuschak), full daily / weekly / monthly / quarterly workflow with exact commands, a real session example, and 7 first-month pitfalls.

### Changed

- **Plugin description** updated everywhere to highlight the **3 skills** the plugin installs: `/onboard-second-brain`, `/open-vault`, `/second-brain`. Beginner-friendly framing — *"three commands, that's the whole app."*
- **SOP-C04-02 rewritten skill-first.** v0.1.0/v0.2.0 had students paste a 200-word manual prompt to run build/update/health-check. v0.3.0 just uses the `/second-brain` skill the plugin installs — students invoke `/second-brain build coaching` and the skill does the work. Manual prompt templates moved to an appendix for reference only. Lesson time: ~15 min (was 30).
- **SOP-C04-04 rewritten — dropped the custom-skill build.** v0.1.0 had students build a "Vault Wiki Workflow" custom skill via Skill Creator. The plugin already provides `/second-brain` (and now `/open-vault`) — building another skill is duplicate work. New Lesson 4 focuses on calendar reminders, home indexes, golden output, `My Vault System.md`, and establishing `/open-vault` as the daily-startup pattern. Lesson time: ~20 min (was 30).
- **SOP-C04-00 (Overview)** updated to list all 3 skills explicitly + 6-doc map (overview + 5 lessons).
- **SOP-C04-01 (Lesson 1)** mentions `/open-vault` as the daily-driver users will adopt after onboarding.

### Why this matters

In v0.1.0/v0.2.0 the SOPs taught the *manual* approach (paste prompts, build your own skill) because the original SOPs were authored before the plugin existed. v0.3.0 reconciles the SOPs with the plugin — the plugin already does the work, the SOPs now teach using it. Beginner-friendly. Less duplicate effort. Mirrors the `cowork-ai-os` install pattern (install once, get a small set of skills the plugin owns).

### Migration from v0.2.0

- **Plugin install command unchanged:** `/plugin install cowork-obsidian@cowork-obsidian` (still works on the existing GitHub repo).
- **Existing slash commands preserved:** `/onboard-second-brain` and `/second-brain` work exactly as before (with expanded natural-language triggers).
- **New `/open-vault` skill auto-installs** when the plugin updates. No user action needed.
- **`vaults.md` and state file format unchanged** from v0.2.0. No migration required.
- **SOPs renumbered:** v0.2.0 had 5 SOPs (00–04). v0.3.0 has 6 (00–05). Old SOP files retained, content updated; new SOP-C04-05 added.

---

## [0.2.0] — 2026-05-09

### Added

- **Renamed:** `cowork-second-brain` → `cowork-obsidian` (plugin + repo). Concept name "second brain" stays. Slash commands `/onboard-second-brain` and `/second-brain` keep their names — published cohort SOPs reference them and muscle memory matters.
- **Phase 0.5 (NEW)** — *Connect existing OR create new.* Auto-detects existing Obsidian vaults from `obsidian.json` (Mac: `~/Library/Application Support/obsidian/obsidian.json`; Windows: `%APPDATA%\obsidian\obsidian.json`; Linux: `~/.config/obsidian/obsidian.json`). Lets the user pick an existing vault to connect to OR fall through to Phase 1 to scaffold fresh.
- **Three vault modes** — every vault now has a `mode` field in `vaults.md`:
  - `external` — connect to existing vault, never reorganize, every write asks permission
  - `overlay` — connect to existing vault, add `raw/wiki/output` to one sub-folder
  - `scaffold` — build from scratch (the v0.1.0 default behavior)
- **Phase 6 (NEW, optional) — Sync across machines.** Decision-tree across Obsidian Sync ($5–10/mo, official, AES-256 E2EE, mobile included), Syncthing (free, peer-to-peer), and Git/GitHub via Obsidian Git plugin (free, version history). First-device-wins rule and unrecoverable-E2EE-password warning baked in. Cohort default: Obsidian Sync.
- **Phase 7 (NEW, optional) — Claude Desktop MCP.** Wires the separate Claude Desktop chat app to the same vault via `iansinnott/obsidian-claude-code-mcp`. Opens with the **GoHighLevel Mac install trap** warning: `~/.claude/settings.json` and `~/.claude/mcp.json` do **not** work for Claude Desktop. The only file that does is `~/Library/Application Support/Claude/claude_desktop_config.json` on Mac.
- **Soft `cowork-ai-os` prerequisite** — Phase 0 detects `cowork-ai-os` v0.7.0+ but doesn't STOP if missing. If absent, ships a self-contained minimal `safe-zones.md` (new template `safe-zones-minimal.template.md`).
- **Mode-aware `/second-brain` operational skill** — reads the vault's `mode` from `vaults.md` and applies different rules per mode. External mode requires per-write user permission.
- **Use-case picker in `/second-brain`** — when invoked with no command, the skill offers a 6-persona menu (Coach / Agency / Course Creator / Consultant / Writer / Multi-Role) and tailors first-run suggestions per persona.
- **Use cases doc** — new `cowork-obsidian/docs/use-cases.md` with 6 case studies (Persona → Pain → raw flow → wiki output → output value → time/value created), drawn from VCInc cohort SOPs.
- **README polish** — badges (license, version, platform, built-for-Cowork), table of contents, "Who this is for" hook, ASCII workflow diagram, collapsible Mac/Windows/Linux install sections.
- **GitHub repo polish** — topics applied (`claude-cowork`, `claude-code`, `claude-plugin`, `obsidian`, `obsidian-vault`, `second-brain`, `pkm`, `personal-knowledge-management`, `local-first`, `productivity`, `vci-inc`); homepage URL set; v0.2.0 release tagged with `cowork-obsidian.zip` asset.

### Changed

- **8 phases instead of 6** (0, 0.5, 1, 2, 3, 4, 5, 6, 7) — but only 0, 0.5, 4, and 5 always run. 1/2/3 only run when scaffolding new. 6 and 7 are optional.
- **Phase 0** — soft prereq checks instead of STOP-at-first-failure. Now redirects to Phase 0.5 instead of straight to Phase 1.
- **Phase 3** — gated to `mode == scaffold` only. External and overlay paths skip Phase 3.
- **Phase 5** — mode-aware. `external` mode requires per-page write permission; default suggestion is `<chosen-input-folder>/wiki/` as a sibling.
- **`onboard-second-brain` SKILL.md** — describes the new branch flow, soft prereqs, and mode awareness. State file migration from v0.1.0 (assumes `vault_mode = scaffold`, prompts for confirmation).
- **`vaults.template.md`** — adds `mode` field, optional `overlay_root` field, mode-reference table.
- **`state-second-brain.template.md`** — adds `vault_mode_default`, `cowork_ai_os`, `cowork_ai_os_p01_complete`, `safe_zones_path`, `phase_6_sync_choice`, `phase_7_desktop_mcp` fields, and Phase 0.5 / Phase 6 / Phase 7 phase status entries.

### Fixed

- `cowork-aibos` typo (the public repo is `cowork-ai-os`) — replaced across all docs (README, RELEASE notes, all phase docs).

### Migration from v0.1.0

- **Plugin install:** `/plugin install cowork-obsidian@cowork-obsidian` (old `cowork-second-brain` install commands still work via GitHub auto-redirect).
- **State file:** existing `_aibos/state-second-brain.md` files are auto-migrated by the new Phase 0 — missing fields inferred (`vault_mode = scaffold`), one confirmation prompt, written back. No data loss.
- **`vaults.md`:** existing entries get `mode: scaffold` appended automatically. Users can change individual vaults to `external` or `overlay` later.
- **Slash commands unchanged:** `/onboard-second-brain` and `/second-brain build|update|health-check|fold-back` work exactly as before.
- **`cowork-ai-os` no longer required:** users waiting on Project 01 to finish can install cowork-obsidian standalone now.

---

## [0.1.0] — 2026-05-08

### Added — initial release (Project 04: Second Brain)

- **`onboard-second-brain` skill** — 6-phase install walkthrough (~70 minutes total, pause-friendly):
  - Phase 0: Welcome + verify cowork-ai-os / Project 01 baseline + scaffold `projects/second-brain/`
  - Phase 1: Plan vaults — interview to capture `vaults.md` config (1 vault for beginners, N for power users)
  - Phase 2: Install Obsidian — manual download walkthrough; auto-detect existing install and skip
  - Phase 3: Scaffold first vault — pick local-drive path (forbid cloud-sync), create life-area structure (raw/wiki/output × N), copy `about-me/` from workspace
  - Phase 4: Wire Cowork to vault — add vault path to `safe-zones.md` as `/second-brain`-scoped carve-out, test by reading a note
  - Phase 5: Three canonical prompts — run build / update / health-check for the first time, establish weekly rhythm
- **`second-brain` operational skill** — wraps the three canonical prompts. Reads `projects/second-brain/vaults.md` to find configured vaults. Operates on the `raw/`, `wiki/`, `output/` subfolders inside each life-area folder of each vault.
  - `build` — first-pass wiki construction from raw notes
  - `update` — fold new raw notes into the existing wiki without rewriting unchanged pages
  - `health-check` — flag contradictions, missing topics, stale pages
- **State file** at `_aibos/state-second-brain.md` — phase tracker, pause/resume support
- **Templates** — `vaults.template.md`, `vault-folder-structure.template.md`, `prompts.template.md`, `state-second-brain.template.md`
- **Sample** — `sample-vaults.md` showing a multi-vault setup (one per context)

### Architecture decisions

- **No MCP server** for v0.1.0. Cowork already has filesystem access; vault path becomes a `/second-brain`-scoped carve-out in `cowork-ai-os`'s `safe-zones.md`. MCP becomes a v0.3.0+ optional feature for richer search.
- **No Local REST API plugin install** required. The plugin reads `.md` files directly from the user's vault folder.
- **No VS Code, no git, no GitHub** required for the cohort path. Power users can add those independently — they're not in the user-facing onboarding.
- **Multi-vault from day one.** `vaults.md` is a list, not a single path.

### Cross-plugin coordination

- Depends on `cowork-ai-os` v0.7.0+ being installed (Project 01 + the safe-zones model).
- The vault carve-out in `safe-zones.md` is the integration point. `cowork-ai-os`'s `/tidy-downloads` will not touch the vault. `/second-brain` only operates on the vault path.
- Future `cowork-ai-os` v0.8.0 will detect `vaults.md` and let `/morning-brief` and `/voice-writer` optionally read from the vault. That cross-skill wiring is deferred — v0.1.0 ships standalone.

### Mac install note

Same model as `cowork-ai-os`. Mac users download `cowork-obsidian.zip` from the [release page](https://github.com/AutomatedMarketer/cowork-obsidian/releases/latest) and re-upload via Settings → Customize → Browse plugins. Windows users use `/plugin install cowork-obsidian@cowork-obsidian` (canonical disambiguated form).
