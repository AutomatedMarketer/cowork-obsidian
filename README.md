# cowork-second-brain

A [Claude Cowork](https://claude.com/product/claude-code) plugin that builds you a local-first **second brain in Obsidian** and wires it directly into Cowork — without MCP, API keys, or the Local REST API plugin.

> **Plain markdown files on your disk. Cowork reads them. Obsidian edits them. You own them forever.**

Built by [Nuno Tavares](https://nunomtavares.com) for [VCInc](https://vcinc.com) cohort students and anyone who wants a knowledge base that compounds for years instead of a chatbot that forgets last Tuesday.

---

## What you get

After ~70 minutes of guided install:

- **Obsidian** installed on your machine (manual download; we walk you through it)
- **One or more local vaults** scaffolded with the [raw / wiki / output](./cowork-second-brain/skills/onboard-second-brain/templates/vault-folder-structure.template.md) folder model
- **Your Cowork `about-me/` files** copied into each vault so identity is consistent everywhere
- **A safe-zones carve-out** in `cowork-aibos` that gives the `/second-brain` skill scoped read/write access to your vault — and nothing else
- **Three canonical prompts** (`build`, `update`, `health-check`) tested and ready to run forever
- **Optional weekly rhythm** — a reminder that fires when it's time to fold new raw notes into your wiki

## What this plugin deliberately doesn't do

- ❌ Install the Obsidian Local REST API community plugin (we don't need it)
- ❌ Set up an MCP server (we don't need it for v0.1.0)
- ❌ Require VS Code or git (cohort path is Cowork-only)
- ❌ Allow your vault to land in iCloud / OneDrive / Dropbox / Google Drive auto-sync folders (Phase 3 forbids these — they corrupt vault writes)
- ❌ Bulk-import your existing notes from another app (push back — pull specific valuable pieces, not whole dumps)

## Prerequisites

- **`cowork-aibos` plugin v0.7.0+** — gives you Project 01 (Core Setup) and the safe-zones model. Get it from [github.com/AutomatedMarketer/cowork-aibos](https://github.com/AutomatedMarketer/cowork-aibos).
- **Project 01 complete** — run `/onboard` from `cowork-aibos` first. Your `about-me/` files are the foundation.
- **A local hard drive** for the vault. Phase 3 forbids cloud-sync paths for good reason.

---

## Install

### Mac (recommended path)

1. Download `cowork-second-brain.zip` from the [latest release](https://github.com/AutomatedMarketer/cowork-second-brain/releases/latest)
2. Open Claude Desktop → click your name (top right) → **Settings**
3. **Customize** → **Browse plugins** → upload the zip
4. Open a fresh Cowork task → run `/onboard-second-brain`

### Windows

```
/plugin install cowork-second-brain@cowork-second-brain
```

Then in a fresh Cowork task:

```
/onboard-second-brain
```

If you've never set up a Cowork plugin marketplace before, run this first:

```
/plugin marketplace add AutomatedMarketer/cowork-second-brain
```

---

## The 6-phase install at a glance

| # | Phase | Time |
|---|---|---|
| 0 | Welcome — verify cowork-aibos v0.7.0+, Project 01, scaffold projects folder | ~5 min |
| 1 | Plan vaults — interview, capture `vaults.md` config (1 vault for beginners, N for power users) | ~10 min |
| 2 | Install Obsidian — download + first launch | ~5 min |
| 3 | Scaffold first vault — local-drive only, life-area folders, copy `about-me/` | ~15 min |
| 4 | Wire Cowork to vault — safe-zones carve-out, read/write test | ~5 min |
| 5 | The three prompts — `build` first wiki, learn `update` and `health-check`, set weekly rhythm | ~30 min |

Pause-friendly. State persists at `_aibos/state-second-brain.md`. Resume any time with `/onboard-second-brain`.

---

## The three canonical prompts

After install, you'll use these forever:

- **`/second-brain build [life-area]`** — first-pass wiki construction from raw notes. Run when wiki/ is empty or sparse.
- **`/second-brain update [life-area]`** — fold new raw notes into the existing wiki. Run weekly.
- **`/second-brain health-check [life-area]`** — flag contradictions, missing topics, stale pages. Run monthly.

Plus an optional ad-hoc:

- **`/second-brain fold-back [output filename]`** — absorb the ideas in a finished output back into the wiki.

See [the prompts reference](./cowork-second-brain/skills/onboard-second-brain/templates/prompts.template.md) for full behavior.

---

## What "raw / wiki / output" means

| Folder | Who writes | What lives there |
|---|---|---|
| `raw/` | **You only** | Brain dumps, journal entries, half-formed ideas, screenshot OCR, article clippings — unpolished thinking |
| `wiki/` | **Claude only** | Short, focused, cross-linked pages (one topic per page) — your distilled understanding |
| `output/` | **Both** | Finished deliverables — strategy docs, client guides, frameworks |

The framing (Julie Chenell): *"There's a human in raw, a robot in wiki, and a human AND a robot in output."*

Over time, raw fills with your thinking. Wiki gets richer. Output accumulates real deliverables. Every Cowork session becomes sharper because Cowork can read everything you've ever written — without you re-uploading anything.

---

## Multi-vault support

Built in from day one. `vaults.md` is a list, not a single path.

Common setups:

- **Beginners:** one vault. Maybe four life areas. Done.
- **Power users:** multiple vaults scoped per context. Nuno runs three (one for Cowork plugin work, one for business, one for personal).

When `/second-brain` is invoked with no argument, it operates on `default_vault`. Pass an argument to target a specific one: `/second-brain business`, `/second-brain personal`.

See the [sample multi-vault config](./cowork-second-brain/skills/onboard-second-brain/samples/sample-vaults.md).

---

## Coordination with `cowork-aibos`

This plugin **depends on** [`cowork-aibos` v0.7.0+](https://github.com/AutomatedMarketer/cowork-aibos) being installed:

- Uses `cowork-aibos`'s `safe-zones.md` mechanism for the vault permission carve-out
- Phase 0 verifies cowork-aibos is present and at the right version

This plugin is **decoupled from** future cowork-aibos releases:

- v0.1.0 ships standalone — you can use the second brain without ever using `/morning-brief` or `/tidy-downloads`
- Future cowork-aibos v0.8.0 will detect `vaults.md` and let `/morning-brief` and `/voice-writer` optionally read from your vault. That cross-skill wiring is deferred — not part of this v0.1.0 release.

---

## Curriculum

Project 04 maps 1:1 to a curriculum module at [vcinc.com](https://vcinc.com) for VCI cohort members. Each phase of the install corresponds to a lesson in the cohort.

---

## License

[MIT](./LICENSE) — yours forever.

## Built by

[Nuno Tavares](https://nunomtavares.com) — Newsletter: [automatedmarketer.net](https://automatedmarketer.net) · YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
