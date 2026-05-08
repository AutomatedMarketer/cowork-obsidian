# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.1.0] — 2026-05-08

### Added — initial release (Project 04: Second Brain)

- **`onboard-second-brain` skill** — 6-phase install walkthrough (~70 minutes total, pause-friendly):
  - Phase 0: Welcome + verify cowork-aibos / Project 01 baseline + scaffold `projects/second-brain/`
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

- **No MCP server** for v0.1.0. Cowork already has filesystem access; vault path becomes a `/second-brain`-scoped carve-out in `cowork-aibos`'s `safe-zones.md`. MCP becomes a v0.3.0+ optional feature for richer search.
- **No Local REST API plugin install** required. The plugin reads `.md` files directly from the user's vault folder.
- **No VS Code, no git, no GitHub** required for the cohort path. Power users can add those independently — they're not in the user-facing onboarding.
- **Multi-vault from day one.** `vaults.md` is a list, not a single path.

### Cross-plugin coordination

- Depends on `cowork-aibos` v0.7.0+ being installed (Project 01 + the safe-zones model).
- The vault carve-out in `safe-zones.md` is the integration point. `cowork-aibos`'s `/tidy-downloads` will not touch the vault. `/second-brain` only operates on the vault path.
- Future `cowork-aibos` v0.8.0 will detect `vaults.md` and let `/morning-brief` and `/voice-writer` optionally read from the vault. That cross-skill wiring is deferred — v0.1.0 ships standalone.

### Mac install note

Same model as `cowork-aibos`. Mac users download `cowork-second-brain.zip` from the [release page](https://github.com/AutomatedMarketer/cowork-second-brain/releases/latest) and re-upload via Settings → Customize → Browse plugins. Windows users use `/plugin install cowork-second-brain@cowork-second-brain` (canonical disambiguated form).
