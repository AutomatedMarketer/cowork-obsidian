# Phase 3 — Scaffold First Vault

**Goal:** Pick a local-drive vault location (forbid cloud-sync paths), create the full folder structure (life areas × raw/wiki/output), copy `about-me/` from workspace into the vault, seed 2–3 raw notes, and point Obsidian at the new vault.

**Time:** ~15 minutes.

---

## Script — what to say (in voice)

> *"Phase 3 builds the empty shell. By the end of this phase, your first vault has the right folder structure, your `about-me/` files are inside it, and Obsidian is pointed at it.*
>
> *Two rules this phase enforces:*
>
> *1. Vault on a local drive. NOT inside OneDrive, iCloud Drive, Dropbox, or Google Drive auto-sync. Cloud sync fights Obsidian's writes and Cowork's writes — you'll get duplicate files, corrupted notes, and phantom disappearing content. Local drive only.*
>
> *2. Same folder structure everywhere. Every life area gets exactly three subfolders: raw, wiki, output. Lowercase. Same spelling. That consistency is what lets the prompts work for every life area forever.*
>
> *Type `let's scaffold` when ready."*

---

## Step 1 — Confirm OS and pick the location

Ask:

> *"What OS are you on? Mac, Windows, or Linux?"*

Capture. Then propose a default safe location:

- **Mac**: `/Users/<username>/SecondBrain/` (or `/Users/<username>/SecondBrain-<vault-name>/` if multi-vault from Phase 1)
- **Windows**: `C:\Users\<username>\SecondBrain\` (NOT `C:\Users\<username>\Documents\` — Documents is OneDrive-synced by default on fresh Windows 11 installs)
- **Linux**: `~/SecondBrain/`

Confirm the path with the user.

### Detect cloud-sync attempts

If the user proposes a path that's inside a known cloud-sync folder, refuse and explain:

| Pattern | What it is | Why no |
|---|---|---|
| `~/Library/Mobile Documents/...` | iCloud Drive | Auto-sync corrupts Obsidian writes |
| `~/Library/CloudStorage/...` | iCloud / OneDrive on Mac | Same |
| `~/OneDrive/` or `C:\Users\<u>\OneDrive\` | OneDrive | Same |
| `~/Dropbox/` | Dropbox | Same |
| `~/Google Drive/` or `G:\My Drive\` | Google Drive | Same |
| `~/Documents/` on fresh Windows 11 | OneDrive-synced by default | Check carefully |

If the user pushes back ("but I want my vault synced!"), explain Obsidian Sync ($5/month, end-to-end encrypted, designed for vaults) is the right tool — and we'll wire that up later if they want, but the vault FOLDER itself stays on a local drive.

---

## Step 2 — Create the folder structure

Read `vaults.md`. For each vault configured:

1. Create the vault root folder (e.g., `/Users/sarah/SecondBrain/`)
2. Update `vaults.md` to fill in the real path (replace the `<TBD — set in Phase 3>` placeholder)
3. For each life area in the vault, create:
   - `<vault-root>/<life-area>/raw/`
   - `<vault-root>/<life-area>/wiki/`
   - `<vault-root>/<life-area>/output/`
4. Create `<vault-root>/about-me/` (the identity files folder at vault root, not inside a life area)

Ask permission before each batch of writes. After each vault is scaffolded, list the resulting tree so the user can visually confirm.

---

## Step 3 — Populate `about-me/` in the vault

Read `vaults.md`'s `about_me_source` setting from Phase 1.

### If `copy_from_workspace`

Copy these files from the user's Cowork workspace into `<vault-root>/about-me/`:
- `about-me/About Me.md` (or whatever the P01 file is named — check the workspace's `about-me/` folder for actual filenames)
- `about-me/writing-rules.md` → save into the vault as `Brand Voice.md`
- `about-me/business-brain.md` (if present)

Check for additional about-me files the user may have created (community-agent.md, etc.) and ask:

> *"You have these additional files in your Cowork about-me/ folder: [list]. Copy these into the vault too? (yes / select / no)"*

### If `fresh`

Interview the user briefly (one question per file) to write three short identity files directly into `<vault-root>/about-me/`:

1. **`About Me.md`** — *"In one paragraph, who are you and what are you working on right now?"*
2. **`Brand Voice.md`** — *"3 quick bullets: tone, what you write about, who you write for."*
3. **`AI Working Style.md`** — *"5–7 bullets: how should I behave when working with you? Hints: no cheerleading, no therapy-speak, assume intelligence, push back when useful, terse over thorough."*

Show drafts; user edits; save when approved.

### Either way — defer `My Vault System.md`

We do NOT create `<vault-root>/about-me/My Vault System.md` in Phase 3. That file documents how the user actually works in the vault — they need to use it for a couple of weeks first. We'll prompt the user to write it ad-hoc later.

---

## Step 4 — Seed 2–3 raw notes

Phase 5 runs the `build` prompt for the first time. It needs material in `raw/` to work with.

Pick one life area (ask the user which). Interview briefly:

> *"What's a half-formed idea, a recent lesson, or a pattern you've noticed in [life area] that you'd want your second brain to remember? Give me 2–3 — they can be short. Don't polish them; raw is for unpolished thinking."*

Capture each as a separate `.md` file inside the life area's `raw/` folder, with descriptive filenames:

- `<vault-root>/<life-area>/raw/2026-05-06-lesson-from-client-call.md`
- `<vault-root>/<life-area>/raw/2026-05-06-pattern-i-noticed-this-week.md`
- `<vault-root>/<life-area>/raw/2026-05-06-half-formed-idea-on-X.md`

Use the date prefix so notes sort chronologically.

---

## Step 5 — Point Obsidian at the new vault

Tell the user:

> *"Now we point Obsidian at the vault we just built.*
>
> *Switch to Obsidian. Click File → Open another vault → Open folder as vault → navigate to [vault path] → click Open.*
>
> *You should see the file tree on the left match what's on disk: about-me/ at top, then your life-area folders, each with raw/wiki/output inside.*
>
> *Confirm what you see."*

If Obsidian shows something different from the disk:

> *"Obsidian's view doesn't match the files on disk. Most likely Obsidian opened a different folder. Walk through Open another vault → Open folder as vault again, and double-check the path."*

---

## Step 6 — Repeat for additional vaults (if multi-vault)

If `vaults.md` has more than one vault configured:

> *"You have [N] vaults to scaffold. We just did [vault-1]. Want to scaffold [vault-2] now, or pause and come back? You can do this incrementally — there's no rush to set up all of them today."*

If yes: loop back to Step 1 with the next vault.
If pause: capture progress in state file, mark `[vault-2] scaffolded: false`, and let the user resume later via `/onboard-second-brain` (it'll detect partial progress and offer to continue).

---

## Verification before advancing

Three checks. Run all three.

### Check 1 — Vault structure on disk matches plan

Open the vault folder in File Explorer / Finder. Confirm:
- `about-me/` exists with at least 3 files (About Me, Brand Voice, AI Working Style)
- Each life area folder exists with exactly three subfolders: `raw/`, `wiki/`, `output/`
- One life area has 2–3 `.md` files in `raw/`
- No other top-level folders (the hidden `.obsidian/` folder Obsidian creates is fine — leave it)

### Check 2 — Obsidian shows the same thing

In Obsidian, the file tree on the left mirrors the disk exactly.

### Check 3 — about-me files have real content

Open each `about-me/*.md` file. Confirm the content is real (not placeholders like *"I am a [role] who [does thing]"*).

---

## Update state and preview Phase 4

Once all three checks pass for the first vault (subsequent vaults can be scaffolded later):

1. Update `_aibos/state-second-brain.md`:
   - `current_phase: 4`
   - `first vault scaffolded: true`
   - In the vault inventory, fill in the real path for the first vault
   - `Phase 3 (scaffold first vault): completed at <ISO timestamp>`
2. Append to `projects/second-brain/memory.md`: *"Phase 3 complete. First vault scaffolded at [path] with [N] life areas. about-me/ populated. [N] raw notes seeded in [life area]."*
3. Tell user:

> *"Phase 3 done. Vault built. Obsidian opens it cleanly.*
>
> *Phase 4 next: we wire Cowork to the vault. Adding the vault path to your `safe-zones.md` as a `/second-brain`-scoped allow-zone. Test by reading a note. About 5 minutes. **No MCP, no API key, no Local REST API plugin.** Just a permissions config update.*
>
> *Type `continue onboarding` when ready."*
