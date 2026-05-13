# Cloud-Sync Detector

> Claude: run this check on every proposed vault path BEFORE scaffolding or connecting. The vault MUST live on a local-only path. Cloud sync corrupts Obsidian vaults — duplicate files, ghost edits, lost work.

## Step 1: Resolve symlinks first

If the proposed path is a symlink, resolve it to its real target. Run the check on the real path, not the symlink.

## Step 2: Check the path string against this table

| Service | The path contains any of |
|---|---|
| iCloud Drive (Mac) | `Library/Mobile Documents/com~apple~CloudDocs/`, `iCloud Drive/`, `Mobile Documents` |
| OneDrive (Win/Mac) | `OneDrive`, `OneDrive - ` |
| Dropbox | `Dropbox` |
| Google Drive (Mac) | `Library/CloudStorage/GoogleDrive-`, `Google Drive/` |
| Syncthing / generic sync | `.stfolder` or `.sync` markers in or above the path |

If ANY match → cloud sync detected → go to Step 3.
If NO match → path is safe → return OK.

## Step 3: Branch by mode

### Scaffold-new mode (user wants a fresh vault)

- Refuse to scaffold at the proposed path
- Tell the user in one sentence: "That folder is in cloud sync. Cloud sync corrupts Obsidian vaults — duplicate files, ghost edits, lost work."
- Offer the safe default:
  - Mac: `~/SecondBrain/`
  - Windows: `C:\Users\<username>\SecondBrain\`
- Ask the user to confirm the safe default OR type a different local path
- Re-run this check on the new path

### Connect-existing mode (user points at an existing vault that's in cloud sync)

- Refuse to wire it as-is
- Tell the user in one sentence: same as above
- Offer to MOVE the vault:
  - Confirm Obsidian is closed (instruct: "Close Obsidian completely before continuing.")
  - Plan the move with a backup: copy first to `<safe-path>`, verify, then delete the cloud-sync original
  - Update Obsidian's vault list config to point at the new path
  - Re-open Obsidian to confirm the vault loads at the new path

### Overlay mode (user wants to add structure to an existing in-cloud vault)

- Same as connect-existing: refuse, offer to move first.

## Step 4: Verify-after-write

After Phase 1 completes (vault path is set), re-run Step 2 on the FINAL path one more time. This catches any sneaky manual override the user typed mid-flow.

If the verify step fails, halt the wizard and surface the failure.
