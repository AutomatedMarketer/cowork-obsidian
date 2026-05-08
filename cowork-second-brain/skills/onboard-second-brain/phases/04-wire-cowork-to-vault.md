# Phase 4 — Wire Cowork to Vault

**Goal:** Add the vault path to `cowork-aibos`'s `safe-zones.md` as a `/second-brain`-scoped carve-out. Test the wiring by reading a note from the vault. No MCP, no API key, no Local REST API plugin.

**Time:** ~5 minutes.

---

## Script — what to say (in voice)

> *"Phase 4 is the connectivity layer. After this phase, I can read and write into your vault folder when you ask the `/second-brain` skill to do something — but `/tidy-downloads` (from cowork-aibos) won't touch the vault, and no other skill writes there either. Scoped permission, not blanket access.*
>
> *The mechanism is simple: we add an entry to your `safe-zones.md` that says 'this path is allow-listed for the /second-brain skill ONLY.' That's the entire wiring.*
>
> *No MCP server. No Local REST API plugin. No API key. Cowork already has filesystem access — we're just telling it which folder is OK to use for second-brain operations.*
>
> *Type `let's wire it` when ready."*

---

## Step 1 — Read state to determine safe-zones approach

Read `_aibos/state-second-brain.md`. Check `safe_zones_path`:

- `existing` → User has already run `/onboard-file-organization`; `safe-zones.md` exists at `projects/file-organization/safe-zones.md`. We add to it.
- `minimal_in_phase_4` → User skipped `/onboard-file-organization`. We create a minimal `safe-zones.md` here with only the second-brain carve-out.

---

## Step 2A — Existing `safe-zones.md` (Option `existing`)

Read `projects/file-organization/safe-zones.md`.

Find (or create) the section `## Skill-specific allow-zones (for cross-skill coordination)`. Append one entry per configured vault from `vaults.md`:

```markdown
- `<absolute vault path>/` — scoped to `/second-brain` skill ONLY. `/tidy-downloads` and other skills must not touch.
```

Example:

```markdown
- `/Users/sarah/SecondBrain/` — scoped to `/second-brain` skill ONLY. `/tidy-downloads` and other skills must not touch.
- `/Users/sarah/SecondBrain-business/` — scoped to `/second-brain` skill ONLY. `/tidy-downloads` and other skills must not touch.
```

Update the Revision history section:

```markdown
- <ISO date> — Added second-brain vault carve-out(s) via /onboard-second-brain Phase 4.
```

Show the user the diff before saving. Ask for approval.

---

## Step 2B — Create minimal `safe-zones.md` (Option `minimal_in_phase_4`)

User skipped `/onboard-file-organization` so there's no full safe-zones file. Create a minimal one at `projects/file-organization/safe-zones.md`:

```markdown
# Safe Zones — Skill-Specific Allow-Zones (Minimal)

This file exists in minimal form because the user installed `cowork-second-brain` without first running `/onboard-file-organization`. It contains ONLY the carve-outs needed for `/second-brain` to operate. The full safe-zones model (allow-list, forbidden list, "never delete" rule, etc.) is documented at:

https://github.com/AutomatedMarketer/cowork-aibos — run `/onboard-file-organization` to install it.

---

## Skill-specific allow-zones

- `<absolute vault path>/` — scoped to `/second-brain` skill ONLY.

## Forbidden (default — applies to ANY future filesystem skill)

- `**/.git/` — version control metadata
- Cloud-sync folders (iCloud, OneDrive, Dropbox, Google Drive) — never touch unless explicitly allow-listed

## Hard rules (apply to all filesystem operations)

- Never delete files
- If a path matches the forbidden list, stop and report

## Revision history

- <ISO date> — Created minimal version via /onboard-second-brain Phase 4 (user skipped /onboard-file-organization).
```

Append vault entries from `vaults.md` to the "Skill-specific allow-zones" section. Show the user before saving.

Also create `projects/file-organization/` parent folder if missing (we share the directory with the future `/tidy-downloads` install if the user adds it later).

---

## Step 3 — Test the wiring

Pick one of the raw notes seeded in Phase 3.

Tell the user:

> *"Now we test. I'm going to read [filename of one of the seeded raw notes] from your vault — the file you wrote in Phase 3 about [topic]. If the wiring works, I'll show you the content. If something's wrong, we'll see exactly what."*

Read the file. If successful:

> *"Read it cleanly. Here's what it says:*
>
> *[content of the raw note, indented]*
>
> *That confirms: the vault is wired, I can read it, and the safe-zones carve-out is working. The `/second-brain` skill can now operate on this vault."*

If the read fails (permission denied, path not found):

> *"Hit a problem reading [filepath]. Likely cause: [diagnose — path mismatch in vaults.md, file actually doesn't exist, OS-level permission issue]. Let's diagnose."*

Walk through the fix. Common issues:
- Path in `vaults.md` doesn't match disk reality (typo, wrong slash direction on Windows)
- File system permissions (rare on user's own home folder, but happens with corporate-managed laptops)
- Cowork on Mac may need explicit Files-and-Folders permission grant — the OS prompts on first access

---

## Step 4 — Test write capability

Tell the user:

> *"One more test. I'm going to write a tiny test note to `<vault-root>/<one of your life areas>/output/wiring-test.md` to confirm I can write to the vault. Then I'll delete it (the only delete the second-brain skill ever does — and only because we and you both just created this test file). OK?"*

Wait for OK. Write the test file with content like:

```markdown
# Wiring Test — <ISO timestamp>

This file was written by `/onboard-second-brain` Phase 4 to verify Cowork can write to the vault.

If you're reading this in Obsidian and the file appeared without you typing it, the wiring works.

This file will be deleted in 30 seconds.
```

Pause briefly. Confirm with user that the file appeared in their vault (in Obsidian's file tree). Then delete it.

If write fails:
- Check the safe-zones entry has the correct path (case-sensitive on Mac/Linux)
- Check the parent folder exists (Phase 3 should have created it)
- Check OS-level write permission

---

## Update state and preview Phase 5

Update `_aibos/state-second-brain.md`:
- `current_phase: 5`
- `vault path allow-listed in safe-zones.md: true`
- `Phase 4 (wire cowork to vault): completed at <ISO timestamp>`

Append to `projects/second-brain/memory.md`: *"Phase 4 complete. Vault path allow-listed in safe-zones.md. Read test passed: [filename]. Write test passed: wiring-test.md (created and deleted)."*

Tell user:

> *"Phase 4 done. Vault wired. I can read your raw notes and write to wiki/ and output/ when you ask the `/second-brain` skill to do so.*
>
> *Phase 5 next — the payoff. We run the three canonical prompts for the first time. `build` reads your seeded raw notes and constructs an initial wiki. `update` and `health-check` get explained for future use. We establish a weekly rhythm. About 30 minutes.*
>
> *Type `continue onboarding` when ready."*
