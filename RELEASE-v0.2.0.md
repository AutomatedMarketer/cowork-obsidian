# cowork-obsidian v0.2.0

**Date:** 2026-05-09
**Tag:** `v0.2.0`
**Renamed from:** `cowork-second-brain` (v0.1.0)

---

## TL;DR

v0.2.0 turns the cohort-only "scaffold a fresh vault" wizard into a **self-onboarding plugin that any Claude Cowork user can install**, including users who already have an Obsidian vault with their own structure. Plus optional sync setup and an optional Claude Desktop MCP path.

The 6-phase backbone from v0.1.0 still works. We added a branch and two optional tail phases.

---

## What's new

### Renamed: `cowork-second-brain` → `cowork-obsidian`

The plugin is renamed to reflect what it actually is: the plugin that wires Obsidian to Cowork. The "second brain" *concept* and the `/onboard-second-brain` and `/second-brain` slash commands keep their names — they're the verbs you've already learned, and the cohort SOPs have been published using them.

GitHub auto-redirects the old `cowork-second-brain` repo URL to the new `cowork-obsidian` repo URL for years, so existing bookmarks and install commands keep working — but new install instructions use the new name.

Also bundled in this release: fixed the `cowork-aibos` typo across all docs (the actual repo is `cowork-ai-os`).

### Phase 0.5 — Connect existing OR create new

The big new feature. After Phase 0 (Welcome), the wizard now branches:

**Path A — Connect existing vault**
- Auto-detects every Obsidian vault you've ever opened by parsing Obsidian's own `obsidian.json`:
  - Mac: `~/Library/Application Support/obsidian/obsidian.json`
  - Windows: `%APPDATA%\obsidian\obsidian.json`
  - Linux: `~/.config/obsidian/obsidian.json`
- Lists them with display names + paths, flags the currently-open one
- Falls back gracefully to manual path entry if detection fails
- Connects in `external` mode — **never reorganizes your vault**
- Optional: offers to add a `raw/wiki/output` overlay on one sub-folder (`overlay` mode)
- Skips Phases 1–3 entirely (you're already set up)

**Path B — Create new vault**
- The original v0.1.0 flow: Phase 1 (plan) → Phase 2 (install Obsidian) → Phase 3 (scaffold)

### Three vault modes

Every vault registered in `vaults.md` now gets a `mode` field that drives the operational `/second-brain` skill's behavior:

| Mode | When | What's created on disk | What `/second-brain` may write |
|---|---|---|---|
| `external` | You picked an existing vault, no overlay | Nothing — we don't touch your structure | Only paths you explicitly name in a command. Never auto-creates folders. Reads everything except `.obsidian/` and `about-me/`. |
| `overlay` | You picked an existing vault and added raw/wiki/output to one sub-folder | Just `<chosen-subfolder>/{raw,wiki,output}/` | Strict raw/wiki/output rules, scoped to the overlay sub-folder. |
| `scaffold` | You had no vault — built from scratch | Full v0.1.0 layout: `<vault-root>/<life-area>/{raw,wiki,output}/ × N` + `<vault-root>/about-me/` | Current v0.1.0 strict rules. |

### Soft `cowork-ai-os` prerequisite

v0.1.0 stopped dead if `cowork-ai-os` wasn't installed. v0.2.0 *detects* `cowork-ai-os`. If found at v0.7.0+ → integrate with its `safe-zones.md`. If not → ship a self-contained minimal `safe-zones.md`. Standalone for new customers; cohort path keeps working.

### Phase 6 (NEW, optional) — Sync across machines

A fully-written phase script with a 2-question decision tree (mobile? cost?) routing to one of three options:

- **Obsidian Sync** ($5/mo Standard or $10/mo Plus) — official, AES-256 E2EE, mobile included, Cure53 audited. Lead recommendation for non-dev cohort users. Includes E2EE password warning ("unrecoverable — store in password manager") and the first-device-wins rule.
- **Syncthing** (free) — peer-to-peer, no cloud, no monthly fee. Per-device pairing.
- **Git/GitHub via Obsidian Git plugin** (free) — auto-commit & push to a private GitHub repo, full version history. Walks `gh repo create` + Obsidian Git plugin install.

**Cloud-sync (iCloud / Dropbox / OneDrive / Google Drive) for the vault folder remains forbidden** — Phase 6 is not a back door to that rule.

### Phase 7 (NEW, optional) — Claude Desktop MCP

Only run if the user *also* wants their separate Claude Desktop chat app to read the same vault. Opens with the Mac install trap (drawn from the GoHighLevel SOP):

> "Most guides will tell you to edit `~/.claude/settings.json` or `~/.claude/mcp.json`. Those don't work for Claude Desktop. The only file that does is `~/Library/Application Support/Claude/claude_desktop_config.json` on Mac (or `%APPDATA%\Claude\claude_desktop_config.json` on Windows)."

Walks the install of [`iansinnott/obsidian-claude-code-mcp`](https://github.com/iansinnott/obsidian-claude-code-mcp), then verifies via Claude Desktop → Customize → Connectors.

### Use cases / case studies

Three locations:

1. **README "Who this is for" hook** — six 1-line persona hooks at the top of the README
2. **Full case studies doc** at [`cowork-obsidian/docs/use-cases.md`](./cowork-obsidian/docs/use-cases.md) — six 150-word case studies (Agency / Coach / Course Creator / Consultant / Writer / Multi-Role)
3. **Embedded in `/second-brain` skill** — when you ask the skill *"what should I use this for?"*, it offers to match a case study to your role and tailor first-run suggestions

### GitHub polish

- Topics: `claude-cowork`, `claude-code`, `claude-plugin`, `obsidian`, `obsidian-vault`, `second-brain`, `pkm`, `personal-knowledge-management`, `local-first`, `productivity`, `vci-inc`
- README badges (license, version, platform, built-for-Cowork)
- Table of contents with jump-links
- Workflow diagram (raw → wiki → output) inline
- Mac/Windows install paths in collapsible `<details>` sections
- Homepage URL set

---

## Migrating from v0.1.0

If you're already running v0.1.0:

- **Plugin name:** the install command is now `/plugin install cowork-obsidian@cowork-obsidian` (old `cowork-second-brain` install commands still work via GitHub's auto-redirect, but please update your saved commands).
- **Existing state file:** `_aibos/state-second-brain.md` is preserved. Phase 0 in v0.2.0 detects v0.1.0-style state with no `vault_mode` field, infers `vault_mode = scaffold` (the v0.1.0 default), prompts you once for confirmation, writes the new fields back. No data loss.
- **`vaults.md`:** existing entries get a `mode: scaffold` field appended automatically. You can change individual vaults to `external` or `overlay` later if you want.
- **Slash commands unchanged:** `/onboard-second-brain` and `/second-brain build|update|health-check|fold-back` work exactly as before.
- **`cowork-ai-os` no longer required:** if you've been waiting on Project 01 (Core Setup) to finish before installing this, you can install cowork-obsidian standalone now.

---

## Install

### Mac

1. Download `cowork-obsidian.zip` from the [latest release](https://github.com/AutomatedMarketer/cowork-obsidian/releases/latest)
2. Open Claude Desktop → **Settings** → **Customize** → **Browse plugins** → upload the zip
3. Open a fresh Cowork task → run `/onboard-second-brain`

### Windows

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

---

## What stays the same

- 6-phase backbone with `_aibos/state-second-brain.md` for pause/resume
- Three canonical prompts: `build`, `update`, `health-check`. Plus `fold-back`.
- Hard rules in the `/second-brain` SKILL.md — never delete a wiki page, never write to `raw/`, scoped to configured vault paths only, halt on `STOP`.
- Cloud-sync forbidden list for the vault folder location (iCloud Drive, OneDrive, Dropbox, Google Drive, `~/Documents` on fresh Win11)
- Multi-vault support via `vaults.md`
- Mac + Windows + Linux paths
- The "second brain" *concept* — and Julie Chenell's *"there's a human in raw, a robot in wiki, and a human AND a robot in output"*

---

## What's still deliberately not in scope

- Claude Desktop as a *first-class* target (Phase 7 stays optional)
- Native Obsidian plugin authoring (cowork-obsidian stays a Cowork plugin only)
- A standalone CLI installer (`npx cowork-obsidian init`) — defer to v0.3.0 if demand surfaces
- Bulk import from other note apps (Notion / Evernote / Apple Notes) — pull specific valuable pieces, not whole dumps
- Vault encryption beyond Obsidian Sync's built-in E2EE

---

## License

[MIT](./LICENSE) — yours forever.

## Built by

[Nuno Tavares](https://nunomtavares.com) — Newsletter: [automatedmarketer.net](https://automatedmarketer.net) · YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
