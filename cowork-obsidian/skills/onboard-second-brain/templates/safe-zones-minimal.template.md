# Safe Zones — Skill-Specific Allow-Zones (Minimal)

This file exists in **minimal form** because the user installed `cowork-obsidian` standalone — without first running `/onboard-file-organization` from `cowork-ai-os`. It contains ONLY the carve-outs needed for `/second-brain` to operate.

If you later install `cowork-ai-os` and run `/onboard-file-organization`, that skill will read this file and merge its full safe-zones model on top — preserving these existing carve-outs.

The full safe-zones model (allow-list, forbidden list, "never delete" rule, etc.) is documented at:

https://github.com/AutomatedMarketer/cowork-ai-os — run `/onboard-file-organization` to install it.

---

## Skill-specific allow-zones

The `/second-brain` skill is allowed to read and write inside these paths. No other skill should touch them.

<!-- One entry per configured vault. Phase 4 of /onboard-second-brain populates this. -->

- `<absolute vault path>/` — scoped to `/second-brain` skill ONLY.

<!--
Example after Phase 4 runs for two vaults:

- `D:/dev/MyVault/` — scoped to `/second-brain` skill ONLY.
- `/Users/sarah/SecondBrain/` — scoped to `/second-brain` skill ONLY.
-->

---

## Forbidden (default — applies to ANY future filesystem skill)

These paths are NEVER touched, even if a skill thinks it has permission elsewhere:

- `**/.git/` — version control metadata
- `**/.obsidian/` — Obsidian's own settings inside any vault
- Cloud-sync folders (iCloud, OneDrive, Dropbox, Google Drive auto-sync paths) — never touch unless explicitly allow-listed and you know what you're doing

---

## Hard rules (apply to all filesystem operations)

- **Never delete files.** Mark stale, flag for user review, or rename — never delete.
- **Never write outside an allow-zone.** If a path is not in the allow-list above, stop and report.
- **Always ask permission for batch writes.** A "batch" is more than 5 files in one operation, or any file with destructive content.
- **Forbidden list overrides allow-list.** A path under both is treated as forbidden.

---

## Revision history

- `<ISO date>` — Created minimal version via `/onboard-second-brain` Phase 4. User opted not to install full `cowork-ai-os` file-organization onboarding.

<!-- Append a one-line dated entry whenever this file changes. -->
