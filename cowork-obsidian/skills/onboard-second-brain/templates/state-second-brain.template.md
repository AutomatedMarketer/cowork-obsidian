# Second Brain — Install State

project: second-brain
version: v3 (P04 / cowork-obsidian v0.4.0)
plugin: cowork-obsidian
plugin_version: 0.4.0
started_at: <ISO timestamp>
current_phase: 0
second_brain_complete: false

## Soft prereq snapshot (captured during Phase 0)
- cowork_ai_os: <detected | detected_old | not_detected>
- cowork_ai_os_p01_complete: <true | false>
- safe_zones_path: <existing | minimal_in_phase_4>
- obsidian_companion_skills: <installed | already_installed | declined | not_checked>   # captured in Phase 4 Step 5

## Vault mode default (captured during Phase 0.5)
- vault_mode_default: <external | overlay | scaffold>

## Optional phase choices
- phase_6_sync_choice: <obsidian_sync | syncthing | git_github | skipped>
- phase_7_desktop_mcp: <enabled | skipped>

## Phase status
- Phase 0 (welcome): not_started
- Phase 0.5 (connect or create): not_started
- Phase 1 (plan vaults): not_started   # may be skipped if path A in Phase 0.5
- Phase 2 (install obsidian): not_started   # may be skipped if path A
- Phase 3 (scaffold first vault): not_started   # may be skipped if path A
- Phase 4 (wire cowork to vault): not_started
- Phase 5 (three prompts): not_started
- Phase 6 (sync setup): not_started   # optional
- Phase 7 (claude desktop mcp): not_started   # optional

## Configured artifacts
- vaults.md: false
- obsidian installed: unknown
- vault registered (external) OR scaffolded (scaffold) OR overlay created (overlay): false
- vault path allow-listed in safe-zones.md: false
- companion skills installed (obsidian:*): <true | false | declined>
- first build run: false
- weekly rhythm established: false
- sync configured: false
- desktop mcp configured: false

## Vault inventory
<!-- Populated in Phase 0.5 (if connecting existing) or Phase 1 (if creating new); one entry per configured vault -->
- (none yet)

## Phase log
<!-- Phase completion entries appended here -->
