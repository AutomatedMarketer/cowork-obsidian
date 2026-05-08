# Phase 2 — Install Obsidian

**Goal:** Confirm Obsidian is installed (or walk the user through installing it). Detect existing installs and skip the install step. Set up the user to pick a vault location in Phase 3.

**Time:** ~5 minutes (less if Obsidian is already installed).

---

## Script — what to say (in voice)

> *"Phase 2 is the Obsidian install. Three possibilities:*
>
> *1. You already have Obsidian installed. Great — we just verify and move on.*
> *2. You don't have it. We download from obsidian.md, run the installer, launch.*
> *3. You're not sure. I'll help you check.*
>
> *Which one are you?"*

---

## Step 1 — Detect existing install

Ask the user to check:

> *"Open your Applications folder (Mac: /Applications) or your Start menu (Windows). Look for 'Obsidian.' Is it there?"*

If yes → skip to Step 4 (verify it launches).
If no or not sure → Step 2.

---

## Step 2 — Walk through the install

> *"Open your browser. Go to https://obsidian.md*
>
> *Click the Download button — it'll detect your OS and offer the right installer (.dmg for Mac, .exe for Windows).*
>
> *Run the installer. Accept defaults. The whole thing is free; no account needed; no payment screen.*
>
> *Tell me when the installer finishes."*

Wait for confirmation. If anything goes wrong (download blocked, installer fails, OS warning about unsigned developer):

> *"Common gotchas:*
> *- Mac sometimes blocks the installer because Obsidian's signed with a developer ID it doesn't recognize on first run. Right-click the .dmg → Open → Open Anyway in System Settings → Privacy.*
> *- Windows sometimes flags the installer with SmartScreen. Click 'More info' → 'Run anyway.' Obsidian is well-known and safe.*
> *- Corporate-managed laptops sometimes block installs entirely. If yours does, you'll need IT to whitelist Obsidian or use a personal machine."*

---

## Step 3 — First launch

> *"Open Obsidian. On first launch it'll show a welcome dialog with two options:*
>
> *- 'Open folder as vault'*
> *- 'Create new vault'*
>
> ***Don't pick either yet.*** *We'll do that in Phase 3. Just confirm the welcome dialog appeared.*
>
> *If you accidentally clicked through and it's showing you the demo vault, that's fine — we'll redirect to your real vault in Phase 3. Just tell me."*

---

## Step 4 — Verify Obsidian is functional

Whether they had it already or just installed, confirm:

- Obsidian launches without errors
- The welcome dialog (or the main app window) is visible
- They can minimize / restore the window

If anything looks broken:

> *"Tell me what you're seeing — error message, blank window, frozen launch — and I'll diagnose."*

---

## Step 5 — Optional: Obsidian Sync (warn but don't enable)

> *"Quick heads-up: Obsidian has a paid Sync feature ($5/month) that syncs your vault across devices end-to-end encrypted. We will NOT enable it during this onboarding. Reasons:*
> *1. We're focused on the local-first install today.*
> *2. If you turn it on later, the device you set up FIRST becomes the source — so do initial setup on your main machine before adding others.*
> *3. The encryption password is unrecoverable if lost. Use a password manager.*
>
> *If you want Sync later, you can enable it from Obsidian Settings → Core plugins → Sync. Not today."*

---

## Update state and preview Phase 3

Update `_aibos/state-second-brain.md`:
- `current_phase: 3`
- `obsidian installed: true`
- `Phase 2 (install obsidian): completed at <ISO timestamp>`

Append to `projects/second-brain/memory.md`: *"Phase 2 complete. Obsidian verified."*

Tell user:

> *"Phase 2 done. Obsidian is alive on your machine.*
>
> *Phase 3 next: we scaffold your first vault — pick a folder location (NOT inside iCloud / OneDrive / Dropbox), create the life-area structure (raw/wiki/output × N), and copy your `about-me/` files in. About 15 minutes.*
>
> *Type `continue onboarding` when ready."*

---

## Verification before advancing

- Obsidian is installed
- Obsidian launches (you don't need to confirm a vault is open yet — Phase 3 handles that)
- The user is at the welcome dialog (or has Obsidian's main window visible)
