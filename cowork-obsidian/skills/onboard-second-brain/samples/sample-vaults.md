# Vaults — Second Brain Configuration

The `/second-brain` skill loads this file to discover where the user's vaults live and what life areas each one contains.

This is **Sarah Mitchell's** filled-in example — three vaults, scoped per context. Most users will start with just one. Sarah is a power user.

---

## Vaults

### `personal`
- **Path:** `/Users/sarah/SecondBrain-personal/`
- **Purpose:** personal life, health, creative side projects, family stuff
- **Life areas:**
  - `health`
  - `home`
  - `creative`
  - `relationships`

### `business`
- **Path:** `/Users/sarah/SecondBrain-business/`
- **Purpose:** Sarah's coaching practice — clients, offers, content, team
- **Life areas:**
  - `clients`
  - `offers`
  - `content`
  - `team`

### `learning`
- **Path:** `/Users/sarah/SecondBrain-learning/`
- **Purpose:** courses Sarah is taking, books she's reading, frameworks she's studying
- **Life areas:**
  - `courses`
  - `books`
  - `frameworks`

---

## Defaults

- `default_vault: business` — Sarah lives in business mode most of the day; default behavior should target it. She invokes `/second-brain personal` or `/second-brain learning` explicitly when needed.

---

## about-me handling

- `about_me_source: copy_from_workspace` — Sarah's original `about-me/` files (from Project 01) were copied into each vault's `about-me/` during Phase 3. The `business` vault's `Brand Voice.md` was edited slightly to add coaching-specific language.

---

## Revision history

- 2026-05-06 — Created. Phase 1 of `/onboard-second-brain`. Three vaults configured.
- 2026-05-06 — Phase 3 filled in real paths after vault scaffolding.
- 2026-05-08 — Added `relationships` life area to `personal` vault (originally three areas, expanded to four).
