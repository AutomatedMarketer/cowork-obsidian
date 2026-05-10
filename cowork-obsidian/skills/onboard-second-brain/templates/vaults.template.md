# Vaults — Second Brain Configuration

The `/second-brain` skill loads this file to discover where the user's vaults live, what mode each is in, and what life areas each one contains. Edit only when adding/removing vaults, switching a vault's mode, or changing life areas.

This file cross-references the `safe-zones.md` for permission scoping — every path listed here must also have a `/second-brain`-scoped allow-zone entry in `safe-zones.md` (either `cowork-ai-os`'s full version or the minimal version shipped with cowork-obsidian).

---

## Vaults

### <vault-name>  (e.g., `personal`, `business`, `client-work`)
- **path:** `<absolute path to vault root>` (e.g., `/Users/<you>/SecondBrain/` or `D:/dev/MyVault/`)
- **mode:** `<external | overlay | scaffold>`
- **purpose:** `<one short sentence>`
- **life_areas:** `[<area-1>, <area-2>, ...]` (lowercase, hyphens; empty `[]` for external mode)
- **overlay_root:** `<absolute path to overlay sub-folder>` *(only if mode = overlay)*

<!-- Add more vault sections below if multi-vault. Three vaults max for v0.1.0/v0.2.0 — more is overkill. -->

---

## Mode reference

| Mode | When | What `/second-brain` does |
|---|---|---|
| `external` | User picked an existing vault, no overlay imposed | Reads everything (except `.obsidian/`, `about-me/`); writes only to paths the user explicitly names per command. Never auto-creates folders. |
| `overlay` | User picked an existing vault and added raw/wiki/output to one sub-folder | Strict raw/wiki/output rules, scoped to `overlay_root`. Treats it like a scaffold-mode vault, but only that sub-folder. |
| `scaffold` | User had no vault and we built one from scratch | Strict v0.1.0 rules. Writes silently allowed inside `wiki/` and `output/` of any configured life area. |

---

## Defaults

- `default_vault: <vault-name>` — the vault used when `/second-brain` is invoked with no argument

---

## about-me handling (scaffold + overlay only)

- `about_me_source: copy_from_workspace` (or `fresh`)

If `copy_from_workspace`: the vault's `about-me/` folder was populated by copying from `<workspace>/about-me/` in Phase 3.
If `fresh`: vault-specific identity files were written from scratch in Phase 3.

External-mode vaults don't get an `about-me/` folder created — we don't touch the existing vault's structure.

---

## Revision history

- `<ISO date>` — Created. Phase 1 (or Phase 0.5 for external/overlay) of `/onboard-second-brain`.

<!-- Append a one-line dated entry whenever this file changes. -->
