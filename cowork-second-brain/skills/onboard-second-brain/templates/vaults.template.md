# Vaults — Second Brain Configuration

The `/second-brain` skill loads this file to discover where the user's vaults live and what life areas each one contains. Edit only when adding/removing vaults or life areas.

This file cross-references `cowork-aibos`'s `safe-zones.md` for permission scoping — every path listed here must also have a `/second-brain`-scoped allow-zone entry in `safe-zones.md`.

---

## Vaults

### <vault-name> (e.g., `personal`, `business`, `client-work`)
- **Path:** `<absolute path to vault root>` (e.g., `/Users/<you>/SecondBrain/`)
- **Purpose:** <one short sentence — e.g., "personal life and side projects">
- **Life areas:**
  - `<area-1>` (e.g., `health`)
  - `<area-2>` (e.g., `home`)
  - `<area-3>` (e.g., `creative`)

<!-- Add more vault sections below if multi-vault. Three vaults max for v0.1.0 — more is overkill. -->

---

## Defaults

- `default_vault: <vault-name>` — the vault used when `/second-brain` is invoked with no argument

---

## about-me handling

- `about_me_source: copy_from_workspace` (or `fresh`)

If `copy_from_workspace`: the vault's `about-me/` folder was populated by copying from `<workspace>/about-me/` in Phase 3.
If `fresh`: vault-specific identity files were written from scratch in Phase 3.

---

## Revision history

- `<ISO date>` — Created. Phase 1 of `/onboard-second-brain`.

<!-- Append a one-line dated entry whenever this file changes. -->
