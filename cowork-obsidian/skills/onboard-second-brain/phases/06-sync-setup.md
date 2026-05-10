# Phase 6 — Sync Across Machines (Optional)

**Goal:** Help the user enable sync of their vault across multiple computers (and optionally mobile). Three approved options. Decision-tree picks the right one based on 2 questions. **Cloud-sync of the vault folder remains forbidden — Phase 6 is not a back door to that rule.**

**Time:** ~10–15 minutes (depending on chosen option).

**Optional:** Skip if user only uses one machine. Set `phase_6_sync_choice: skipped` and advance to Phase 7 or finish.

---

## Script — what to say (in voice)

> *"Phase 6 is optional. Skip it if you only ever work on one computer. Otherwise, we set up sync so the same vault is available on all your machines.*
>
> *Three approved options. Two questions narrow it down:*
>
> *1. Will you also use this on iPad / iPhone / Android?*
> *2. Cost-sensitive (free) or want the easiest path (paid)?*
>
> *Then I'll walk you through the chosen option step by step. About 10–15 minutes."*

---

## Step 1 — The 2-question decision tree

Ask Q1, then Q2. Map answers to one of three options:

```
Q1: Will you also use this on iPad / iPhone / Android?
Q2: Free or paid?

  mobile yes + paid OK     → Obsidian Sync         (Step 2 below)
  mobile no + free + tech  → Syncthing             (Step 3 below)
  free + already on GitHub → Obsidian Git          (Step 4 below)
  single machine for now   → skip Phase 6, return later via "/onboard-second-brain"
```

**Default recommendation for VCInc cohort users:** Obsidian Sync. Reason — easiest setup, one-click after install, mobile included, paid for by *the user*'s subscription (not the agency's), end-to-end encrypted with Cure53 audit. The non-dev path.

If the user is unsure or hesitates, lead with Obsidian Sync and tell them exactly that.

---

## Step 2 — Obsidian Sync route (paid, easiest)

**Tell the user up front:**

> *"Obsidian Sync is Obsidian's official paid service. It's $4/mo (annual) or $5/mo (monthly) for Standard, $8/mo (annual) or $10/mo (monthly) for Plus. Standard fits most users (1 vault, 1 GB storage, 5 MB file cap, 1-month version history). Plus is for power users (10 vaults, 10 GB → upgradable to 100 GB, 200 MB file cap, 12-month history).*
>
> *Two important rules before we start:*
>
> *1. **The first device you enable Sync on becomes the source of truth** for the remote copy. We do this on your main / most-used machine first. If you set it up on a laptop and then add your desktop, the desktop will sync DOWN the laptop's vault state — which is probably not what you want. So pick the machine where the vault is most complete now.*
>
> *2. **The end-to-end encryption password is unrecoverable.** If you lose it, you lose access to your synced vault on every new device — by design. Cure53 audited; AES-256. We'll set the password and store it in your password manager (1Password, Bitwarden, etc.) **immediately**, before going further."*

### Mac + Windows setup (identical steps)

1. Open Obsidian. Settings → Core plugins.
2. Search for **"Obsidian Sync"** → toggle it on.
3. Settings → Obsidian Sync.
4. **Sign in** to your Obsidian account. (Free Obsidian account required first; sign up at https://obsidian.md/account if you don't have one.)
5. **Choose or create a Sync subscription** — Standard or Plus.
6. **Set the End-to-End Encryption password.** Pause here. Walk the user through:
   - Open password manager
   - Generate or pick a strong password
   - Save it under entry name *"Obsidian Sync E2EE — <vault name>"*
   - Note in the entry: *"Unrecoverable. Used to encrypt vault contents before they leave my device."*
   - Confirm saved before pasting into Obsidian
7. Choose the vault to sync (the vault you're set up in via cowork-obsidian Phases 0.5 or 3).
8. Wait for the initial upload.

### On the second device

1. Install Obsidian (Phase 2 instructions if needed).
2. Open Obsidian → Settings → Core plugins → enable **Obsidian Sync** → Settings → Obsidian Sync → Sign in (same account).
3. **Choose "Receive a vault"** (not "Send" — sending would overwrite the remote with this device's empty vault).
4. Pick the same vault name → Obsidian downloads it locally.
5. The E2EE password decrypts on this device — paste from the password manager.
6. Done. Edits made on either machine sync to the other within seconds when both are online.

### Conflict resolution

If two devices edit the same note offline and then both come online:
- Obsidian creates `Note (conflict copy).md` containing one of the versions
- The original `Note.md` keeps the other version
- User decides which to keep (or merges manually)

This is the same on every option, but worth showing the file naming so users aren't surprised.

---

## Step 3 — Syncthing route (free, peer-to-peer)

**Tell the user up front:**

> *"Syncthing is free, open-source, peer-to-peer file sync. No cloud, no monthly fee, files never enter someone else's servers. The trade-off: per-device pairing setup (~5 min per device) and mobile support is harder than Obsidian Sync.*
>
> *Best fit if you have 2–3 desktops/laptops and don't need mobile."*

### Mac install

```
brew install syncthing
brew services start syncthing
```

Then open `http://localhost:8384` in a browser.

### Windows install

1. Download from https://syncthing.net/downloads/
2. Run the installer; pick the "service" install option for auto-start.
3. Open `http://localhost:8384` in a browser.

### Pairing devices

On both devices:
1. Note the **Device ID** (Actions → Show ID).
2. On device 1: Add Remote Device → paste device 2's ID → name it.
3. On device 2: Accept the incoming pairing request.

### Add the vault folder

On device 1:
1. Add Folder → point at the vault path → set Folder ID (anything memorable).
2. Share with the device 2 you just paired.

On device 2:
1. Accept the incoming folder share.
2. Set the local path where the vault should live on this machine.
3. Sync starts immediately.

### Conflict resolution

Syncthing handles conflicts at the file level — typically renames one copy with `.sync-conflict-...` suffix. Less elegant than Obsidian Sync, but rare in practice if devices are connected often.

---

## Step 4 — Git / GitHub route (free, version history)

**Tell the user up front:**

> *"This route uses Git + a private GitHub repo for sync. It's free, gives you full version history (every save is a commit), and works on every desktop. Mobile is doable but limited — the Obsidian Git plugin uses isomorphic-git (JavaScript) on mobile, has memory constraints, and no SSH support, so don't rely on it as your primary mobile path.*
>
> *Best fit if you already use GitHub for other things and want version control as a side benefit."*

### Step 4.1 — Create the private GitHub repo

```
gh repo create <username>/my-vault --private --description "My Obsidian second brain — synced via Obsidian Git"
```

Replace `<username>` with the user's GitHub handle. Show the user the resulting URL.

### Step 4.2 — Install the Obsidian Git plugin

In Obsidian:
1. Settings → Community plugins → Browse.
2. Search for **"Obsidian Git"** (author: denolehov).
3. Install + enable.

### Step 4.3 — Configure the plugin

Settings → Obsidian Git:
- **Vault backup interval (minutes):** 10 (or whatever cadence the user prefers)
- **Auto pull interval (minutes):** 10
- **Pull updates on startup:** on
- **Commit message:** `vault auto-commit {{date}}` (or default)
- **Disable push:** off
- **Disable pull:** off

### Step 4.4 — Initialize the local git repo

In a terminal at the vault path:

```
cd <vault-path>
git init
git remote add origin git@github.com:<username>/my-vault.git
git add .
git commit -m "initial vault"
git push -u origin main
```

(If using HTTPS instead of SSH, the URL is `https://github.com/<username>/my-vault.git`.)

### Step 4.5 — Test the loop

1. Edit a note in Obsidian.
2. Wait for the auto-commit interval (or trigger manually: Command palette → "Obsidian Git: Commit all changes").
3. Check `https://github.com/<username>/my-vault/commits/main` — the commit should appear.

### Step 4.6 — Set up the second machine

```
gh repo clone <username>/my-vault <vault-path-on-this-machine>
```

Then in Obsidian on this machine: open the cloned folder as a vault. Install + enable Obsidian Git. Same plugin config as device 1.

### Conflict resolution

Git handles conflicts at the file level. If two devices edit the same note before either pushes, you get a merge conflict on pull. The plugin offers a basic conflict view; Git veterans can resolve via CLI. Less newcomer-friendly than Obsidian Sync's conflict copies.

---

## Step 5 — Hard rule: no cloud-sync of the vault folder

The vault folder itself **must remain on a local drive**. Forbidden vault locations (same as v0.1.0 Phase 3):

| Pattern | What it is |
|---|---|
| `~/Library/Mobile Documents/...` | iCloud Drive |
| `~/Library/CloudStorage/...` | iCloud / OneDrive on Mac |
| `~/OneDrive/` or `C:\Users\<u>\OneDrive\` | OneDrive |
| `~/Dropbox/` | Dropbox |
| `~/Google Drive/` or `G:\My Drive\` | Google Drive |
| `~/Documents/` on fresh Windows 11 | OneDrive-synced by default |

These cloud-sync services auto-sync the folder bytes underneath the running Obsidian and Cowork apps. They cause:
- Duplicate files (`Note.md` and `Note 1.md`)
- Corrupted writes (partial syncs while a write is in flight)
- Phantom disappearing content (deletes propagated incorrectly)

**Phase 6's three options are NOT auto-folder-sync:**
- **Obsidian Sync** is application-layer — Obsidian itself manages encryption + transfer
- **Syncthing** is peer-to-peer — it does sync the folder, but with locking and conflict awareness designed for live-edited files
- **Obsidian Git** is version-control — you (or the plugin) commit explicitly; no live byte-sync

If the user asks to *"just put the vault in OneDrive instead, that's easier,"* refuse. The corruption isn't theoretical — it shows up as soon as both devices are open at the same time. Recommend Obsidian Sync as the easy alternative.

---

## Step 6 — Update state and finish

Update `_aibos/state-second-brain.md`:
- `phase_6_sync_choice: obsidian_sync | syncthing | git_github | skipped`
- `Phase 6 (sync setup): completed at <ISO timestamp>`
- If sync was set up: store the relevant detail (e.g., `git_remote: <repo url>`, or `syncthing_devices: <count>`, or `obsidian_sync_subscription: standard | plus`)

Append to `projects/second-brain/memory.md`:

```markdown
## <ISO timestamp> — Phase 6 complete: sync setup
- Choice: <obsidian_sync | syncthing | git_github | skipped>
- Devices configured: <count>
- Notes: <one-line summary, e.g., "Subscribed to Sync Standard. Initial upload complete from main desktop. Second device pulls down later this week.">
```

Tell the user:

> *"Phase 6 done. Your sync is wired.*
>
> *Next: Phase 7 (optional) — set up Claude Desktop to also see your vault via MCP. Skip if you only use Cowork. Type `continue onboarding` to proceed, or `wrap up` to finish."*

---

## Verification before advancing

- The chosen option's setup steps are confirmed working (see per-option verification above).
- State file updated.
- Memory entry appended.

---

## What this phase never does

- Allow the vault folder to be moved into iCloud / Dropbox / OneDrive / Google Drive (cloud-sync corrupts vault writes — same hard rule as v0.1.0 Phase 3)
- Auto-enable any sync option without explicit user consent
- Store the user's E2EE password anywhere except their password manager
- Bypass the first-device-wins rule for Obsidian Sync (must set up on main machine first)
- Push vault contents to a public GitHub repo (Step 4 explicitly uses `--private`)
