# Phase 1 — Plan Vaults

**Goal:** Interview the user, capture `projects/second-brain/vaults.md`. This file lists every vault the second brain operates on — name, path, life areas inside.

**Time:** ~10 minutes.

---

## The principle to state up-front

> *"Phase 1 is the planning conversation. Most people end up with one vault — that works fine. Some power users run multiple, scoped per context (one for business, one for personal, one for a specific project, etc.). The benefit of multiple is privacy and focus — when I'm working in your business vault, I don't see your personal notes. The cost is more setup and more places to write into. We'll pick what makes sense for you.*
>
> *Either way, each vault holds 1–4 life areas. Each life area has three folders: raw, wiki, output. Same shape everywhere. Predictable structure means simpler prompts forever.*
>
> *I'll ask 4 questions. Take your time."*

---

## Section A — Vault count

### A1
*"Are you setting up one vault, or multiple? Most people start with one. Multiple makes sense if you have very different contexts you want to keep separate (e.g., business vs personal vs a specific client project)."*

Capture `vault_count`. If user is unsure, default to **one** — they can add more later by re-running this skill.

### A2 (only if multiple)
*"Got it. How many vaults? And what's each one's purpose? Push back: more than 3 vaults is usually overkill for v0.1. You can always add more later."*

Capture per-vault: `name` (lowercase, hyphens), `purpose` (one short sentence), `path` (we'll fill this in Phase 3).

---

## Section B — Life areas (per vault)

### B1
*"For [each vault], what 1–4 life areas should live inside? A life area is a top-level folder like `business/`, `health/`, `personal/`, `work-life/`. Each one gets its own raw/wiki/output trio."*

Push back if the user lists more than 4 per vault:

> *"More than 4 life areas gets confusing fast — for you and for me. Macro beats micro. Subfolders inside `raw/` are fine for fine-grained topics. Let's pick the 4 highest-level groupings."*

Common combos that work:
- `work / business / health / personal`
- `coaching / agency / writing / personal`
- `consulting / content / learning / home`

Capture per vault: `life_areas: [<name1>, <name2>, ...]` (lowercase, hyphens).

---

## Section C — about-me handling

### C1
*"You already have an `about-me/` folder in your Cowork workspace from Project 01 — About Me, Brand Voice, AI Working Style, etc. In Phase 3 I'll copy those into the vault root so the same identity files are available when I'm working in the vault. Sound good? (Yes / no — write fresh ones in the vault)."*

Default: yes, copy from workspace.

If user wants fresh ones: capture `about_me_source: fresh` and we'll re-interview in Phase 3. Otherwise `about_me_source: copy_from_workspace`.

---

## Section D — Seed content

### D1
*"Phase 5 (the three prompts) needs at least one life area to have 2–3 raw notes in it before the `build` prompt has anything to chew on. After we scaffold the vault in Phase 3, I'll interview you to capture 2–3 short raw notes — half-formed ideas, recent lessons, patterns you've noticed — so Phase 5 has material to work with. Just heads-up that's coming."*

No capture needed; just preparing the user.

---

## Write `vaults.md`

After all sections answered:

Use `templates/vaults.template.md` as the structure. Fill in real values from the user's answers. Show the draft in a code block before saving. Ask the user to verify, edit if needed, then save to `projects/second-brain/vaults.md`.

The file should have:

1. **H1**: "Vaults — Second Brain Configuration"
2. **Intro**: *"The `/second-brain` skill loads this file to discover where the user's vaults live and what life areas each one contains. Edit only when adding/removing vaults or life areas. Cross-references `cowork-aibos`'s `safe-zones.md` for permission scoping."*
3. **H2 "Vaults"** — one subsection per configured vault:
   - Name
   - Path (placeholder until Phase 3 fills it in)
   - Purpose (one line)
   - Life areas (bulleted list)
4. **H2 "Defaults"** — `default_vault: <name>` (the one used when `/second-brain` is invoked with no argument)
5. **H2 "about-me handling"** — `copy_from_workspace` or `fresh`
6. **H2 "Revision history"** — one-line dated entry on creation

---

## Verification before advancing

Two checks:

### Check 1 — vaults.md is valid

Open `projects/second-brain/vaults.md`. Confirm:
- At least one vault is listed
- Each vault has a name (lowercase, no spaces)
- Each vault has 1–4 life areas
- `default_vault` is set
- The "path" field is currently a placeholder (e.g., `<TBD — set in Phase 3>`); Phase 3 fills it in with a real local path

### Check 2 — life-area count is reasonable

If any vault has more than 4 life areas, push back:

> *"This vault has [N] life areas. That's more than recommended. Want to consolidate to 4 macro areas? I'll show you the consolidation."*

---

## Update state and preview Phase 2

Once both verifications pass:

1. Update `_aibos/state-second-brain.md`:
   - `current_phase: 2`
   - `vaults.md: true`
   - `Phase 1 (plan vaults): completed at <ISO timestamp>`
   - Append vault inventory: one line per configured vault (name + life areas count + path: TBD)
2. Append to `projects/second-brain/memory.md`: *"Phase 1 complete. vaults.md created with [N] vault(s) and [M] total life areas."*
3. Tell user:

> *"Phase 1 done. Your vault config is sketched.*
>
> *Phase 2 next: install Obsidian. ~5 minutes. You'll download it from obsidian.md, run the installer, and on first launch I'll help you pick the location for your first vault. If you already have Obsidian installed, even better — I'll detect it and we'll skip the install part.*
>
> *Type `continue onboarding` when ready."*
