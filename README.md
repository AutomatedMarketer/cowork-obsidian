# cowork-obsidian

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](./LICENSE)
[![Version](https://img.shields.io/badge/version-0.5.1-brightgreen.svg?style=flat-square)](./CHANGELOG.md)
[![Platform: Mac · Windows · Linux](https://img.shields.io/badge/platform-Mac%20%C2%B7%20Windows%20%C2%B7%20Linux-blue.svg?style=flat-square)](#install)
[![Built for Claude Cowork](https://img.shields.io/badge/built%20for-Claude%20Cowork-7C3AED.svg?style=flat-square)](https://claude.com/product/cowork)

A [Claude Cowork](https://claude.com/product/cowork) plugin (also runs in [Claude Code](https://claude.com/product/claude-code)) that wires a local-first **Obsidian second brain** directly to Cowork — connect to a vault you already have, or scaffold a brand-new one. Plus optional sync across your machines.

> **Plain markdown files on your disk. Cowork reads them. Obsidian edits them. You own them forever.**

Built by [Nuno Tavares](https://nunomtavares.com) for [VCInc](https://vcinc.com) cohort students and anyone who wants a knowledge base that compounds for years instead of a chatbot that forgets last Tuesday.

---

## Why this exists

In April 2026, Andrej Karpathy published [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), a gist that crystallized the markdown-vault-as-second-brain pattern:

> "Obsidian is the IDE, the LLM is the programmer, the wiki is the codebase. You rarely ever write or edit the wiki manually."

Boris Cherny (Claude Code lead, Anthropic) writes the same idea differently:

> "Anytime we see Claude do something incorrectly we add it to the CLAUDE.md, so Claude knows not to do it next time."

Same pattern, two voices. Both point at the same thing: **a long-term memory the agent can reach into only when needed, persisted in plain markdown, version-controlled, vendor-neutral.** This plugin is the install + scaffold + wiring that makes that practical for Claude Cowork users.

---

## How memory works (the 4 tiers)

| Tier | What | Where | Loaded when | ~Token cost |
|---|---|---|---|---|
| 1 | Identity | `about-me/` (cowork-ai-os) | Skills that need it | 300 |
| 2 | Vault hot cache | `wiki/hot.md` | Start of any vault session | 500 |
| 3 | Vault index | `wiki/_index.md` | When a query needs to pick a topic | 1,000 |
| 4 | Full wiki pages | `wiki/<area>/<topic>.md` | Only when explicitly named or matched | 100–300 each |

A typical vault query touches ~3,500 tokens, not 50,000. The vault stops being a folder and becomes Cowork's long-term memory.

---

## ⚡ 1-minute install

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

Mac users via Claude Desktop's plugin uploader: see [Install (Mac)](#mac) below.

---

## 🎯 The three commands you'll use forever

After install, Cowork has **three new slash commands**. Memorize these — they're the whole "app":

| Command | What it does | When |
|---|---|---|
| **`/onboard-second-brain`** *(or `start onboarding`)* | First-time setup. 9-phase wizard. Auto-detects existing vaults or scaffolds fresh ones. Wires Cowork. Optional sync setup. Optional Claude Desktop MCP. | **Once.** |
| **`/open-vault`** *(or `open my vault`)* | Daily-driver. Launches Obsidian to your vault AND preps the Cowork session. Reports vault status (last activity, new raw notes, what to do next). | **Every session.** |
| **`/second-brain`** *(or `update my wiki`, `build my [area]`)* | Operational skill. Four operations: `build`, `update`, `health-check`, `fold-back`. Mode-aware. The skill knows the prompts — you just invoke. | **Weekly+.** |

That's the whole surface. Three commands. After install, run `/onboard-second-brain` and the wizard walks you through them.

---

## Contents

- [Who this is for](#who-this-is-for)
- [What you get](#what-you-get)
- [How it works (no jargon)](#how-it-works-no-jargon)
- [The install at a glance](#the-install-at-a-glance)
- [The three canonical prompts](#the-three-canonical-prompts)
- [Sync across machines](#sync-across-machines)
- [Multi-vault support](#multi-vault-support)
- [What this plugin deliberately doesn't do](#what-this-plugin-deliberately-doesnt-do)
- [Install](#install)
- [Use cases / case studies](#use-cases--case-studies)
- [Coordination with `cowork-ai-os`](#coordination-with-cowork-ai-os)
- [Curriculum](#curriculum)
- [License](#license)

---

## Who this is for

- **Coaches** — distill 50 client conversations into reusable frameworks
- **Agency owners** — client deliverables that draft themselves from indexed past work
- **Course creators** — evergreen curriculum that compounds across cohorts
- **Consultants** — research that accumulates instead of restarting every engagement
- **Writers / creators** — half-formed ideas that find their way into finished pieces
- **Multi-business operators** — work, business, health, personal in one indexed system

If you've watched ChatGPT forget last Tuesday or your Notion turn into a graveyard, this is the alternative.

[Read the full case studies →](./cowork-obsidian/docs/use-cases.md)

---

## What you get

After ~70 minutes of guided install:

- **Obsidian** detected (or installed) on your machine — Mac, Windows, or Linux
- **Auto-detected existing vaults** — the plugin parses Obsidian's own config to find vaults you already have. Or scaffold a new vault from scratch.
- **One of three vault modes** depending on your situation:
  - `external` — you have a vault with your own structure (PARA, Zettelkasten, daily notes, anything). We just connect to it. We don't reorganize it.
  - `overlay` — you have a vault and want a `raw/wiki/output` subfolder added on top of one area. We add only that.
  - `scaffold` — you have no vault yet. We build the full life-area + raw/wiki/output structure for you.
- **Your Cowork `about-me/` files** copied into each vault so identity is consistent everywhere
- **A safe-zones carve-out** that gives the `/second-brain` skill scoped read/write access to your vault — and nothing else
- **The official `obsidian:` skill pack** (5 skills — `obsidian-markdown`, `obsidian-bases`, `json-canvas`, `obsidian-cli`, `defuddle`) detected or installed in Phase 4 — gives Cowork the *current* Markdown / Bases / Canvas syntax so it authors valid files in your vault (Bases shipped in 2025 and keeps evolving; training-data knowledge is unreliable here). Skip if you already have it or prefer not.
- **Three canonical prompts** (`build`, `update`, `health-check`) tested and ready to run forever
- **Optional sync across machines** — Obsidian Sync ($5/mo, easiest), Syncthing (free, peer-to-peer), or Git/GitHub (free, version history)
- **Optional Claude Desktop MCP** — let your Claude Desktop chat app see the vault too, with the GoHighLevel Mac install trap warnings baked in
- **Optional weekly rhythm** — a reminder that fires when it's time to fold new raw notes into your wiki

---

## How it works (no jargon)

There are two different Claude products. They connect to your vault two different ways. **The main path uses Cowork's filesystem access — no MCP needed.**

| Product | How it sees your vault |
|---|---|
| **Claude Cowork** (= Claude Code) | **Direct filesystem access.** When you point Cowork at a folder, it can read & write every file in that folder using its native tools. No MCP, no API key, no plugin. This is the main path of cowork-obsidian. |
| **Claude Desktop** (the chat app) | **No direct filesystem access by default** — needs an MCP server. cowork-obsidian's optional Phase 7 walks you through that setup, with the trap warnings (the `~/.claude/settings.json` and `~/.claude/mcp.json` files **don't work** for Claude Desktop — only `~/Library/Application Support/Claude/claude_desktop_config.json` does on Mac). |

**Verified live:** the auto-detect parses Obsidian's own `obsidian.json`, lists your vaults, and Cowork reads them via plain filesystem reads. Zero MCP, zero credentials.

---

## The install at a glance

9 phases, ~70 minutes, fully pause-friendly (less if you're connecting to an existing vault). State persists at `_aibos/state-second-brain.md`. Resume any time with `/onboard-second-brain` (or `start onboarding`).

| # | Phase | Time |
|---|---|---|
| 0 | Welcome — soft prereq check (cowork-ai-os if installed; minimal safe-zones if not) | ~5 min |
| 0.5 | **Connect existing OR create new** — auto-detects your existing Obsidian vaults; pick one or scaffold from scratch | ~5–10 min |
| 1 | Plan vaults (only if creating new) — interview, capture `vaults.md` config | ~10 min |
| 2 | Install Obsidian (only if creating new) — auto-detect existing app install + walk download | ~5 min |
| 3 | Scaffold (only if creating new) — local-drive only, life-area folders, copy `about-me/` | ~15 min |
| 4 | Wire Cowork to vault — safe-zones carve-out, read/write test | ~5 min |
| 5 | The three prompts — `build` first wiki, learn `update` and `health-check`, set weekly rhythm | ~30 min |
| 6 | **(Optional) Sync setup** — Obsidian Sync, Syncthing, or Git/GitHub decision tree | ~10 min |
| 7 | **(Optional) Claude Desktop MCP** — let Claude Desktop see your vault, with Mac-trap warnings | ~10 min |

---

## The four operations of `/second-brain`

After install, you'll use these forever via the `/second-brain` skill:

- **`/second-brain build [life-area]`** — first-pass wiki construction from raw notes. Run once per area.
- **`/second-brain update [life-area]`** — fold new raw notes into the existing wiki. Run weekly.
- **`/second-brain health-check [life-area]`** — flag contradictions, missing topics, stale pages. Run monthly.
- **`/second-brain fold-back [life-area]`** — absorb finished outputs back into the wiki. Run quarterly.

The skill is **mode-aware** — operates silently in `scaffold`/`overlay` mode, asks per-write in `external` mode (so we never reorganize a vault you already had).

See [the prompts reference](./cowork-obsidian/skills/onboard-second-brain/templates/prompts.template.md) for the underlying templates (you don't paste these — the skill runs them internally).

---

## What `raw / wiki / output` means

```
       YOU                CLAUDE COWORK            YOU + CLAUDE
        │                       │                       │
        ▼                       ▼                       ▼
      raw/  ──build/update──▶  wiki/  ──reads──▶  conversation  ──save──▶  output/
        ▲                                                                     │
        └─────────────────── fold back in ────────────────────────────────────┘
```

| Folder | Who writes | What lives there |
|---|---|---|
| `raw/` | **You only** | Brain dumps, journal entries, half-formed ideas, screenshot OCR, article clippings — unpolished thinking |
| `wiki/` | **Claude only** | Short, focused, cross-linked pages (one topic per page) — your distilled understanding |
| `output/` | **Both** | Finished deliverables — strategy docs, client guides, frameworks |

The mental model: **There's a human in raw, a robot in wiki, and a human AND a robot in output.**

Over time, raw fills with your thinking. Wiki gets richer. Output accumulates real deliverables. Every Cowork session becomes sharper because Cowork can read everything you've ever written — without you re-uploading anything.

---

## Sync across machines

Phase 6 walks you through one of three options based on a 2-question decision tree (mobile? cost?):

| Option | Cost | What it is | Best for |
|---|---|---|---|
| **Obsidian Sync** *(recommended for cohort)* | $5/mo Standard, $10/mo Plus | Official Obsidian SaaS, AES-256 end-to-end encrypted, mobile included (iOS + Android), version history | Easiest setup; non-dev users; mobile-first |
| **Syncthing** | Free | Peer-to-peer, no cloud, no monthly fee | Privacy-conscious; multi-OS; desktop-only OK |
| **Git / GitHub** *(via Obsidian Git)* | Free | Auto-commit & push to a private GitHub repo, full version history | Already on GitHub; want history; comfortable with version control |

> **Cloud-sync (iCloud / Dropbox / OneDrive / Google Drive) for the vault folder is forbidden** — it corrupts vault writes. Phase 6 is **not** a back door to that rule. The three approved options above are application-layer (Obsidian Sync), peer-to-peer (Syncthing), or version-control (Git) — none of them auto-sync the folder bytes underneath the running app.

---

## Multi-vault support

Built in from day one. `vaults.md` is a list, not a single path. Each vault has a `mode` (`external` / `overlay` / `scaffold`).

Common setups:

- **Beginners:** one vault. Maybe four life areas. Done.
- **Power users:** multiple vaults scoped per context. Nuno runs three (one for Cowork plugin work, one for business, one for personal).
- **Existing-vault users:** connect to vaults you already have without re-organizing them.

When `/second-brain` is invoked with no argument, it operates on `default_vault`. Pass an argument to target a specific one: `/second-brain business`, `/second-brain personal`.

See the [sample multi-vault config](./cowork-obsidian/skills/onboard-second-brain/samples/sample-vaults.md).

---

## What this plugin deliberately doesn't do

- ❌ Install the Obsidian Local REST API community plugin (we don't need it for Cowork)
- ❌ Require an MCP server for the Cowork path (Cowork has filesystem access already; MCP is only for the optional Claude Desktop integration in Phase 7)
- ❌ Force you to use VS Code or git for the cohort path (cohort path is Cowork-only)
- ❌ Allow your vault to land in iCloud / OneDrive / Dropbox / Google Drive auto-sync folders (cloud-sync corrupts vault writes — Phase 3 forbids these as the vault location, Phase 6 doesn't relax it)
- ❌ Force the `raw/wiki/output` overlay onto your existing vault (use `external` mode if you want to keep your structure)
- ❌ Bulk-import your existing notes from another app (push back — pull specific valuable pieces, not whole dumps)

---

## Install

### Mac

## 🍎 ✅ Mac install (recommended): zip upload

> ### ⚠️ Already tried installing this plugin before? Read this first.
>
> Cowork has a known Mac quirk: **if you've previously uploaded a plugin with the same name (even a failed or stale attempt), the next upload is silently rejected** — no error toast, no log line, the new zip just doesn't replace the old one. This happens because Cowork sends `overwrite=false` on every upload and there's no UI affordance to override it.
>
> **Fix before you re-upload (30 seconds):**
> 1. 🗑️ Open Claude Cowork → **Settings → Customize → Plugins** (or **Skills**, depending on Cowork version)
> 2. Find any existing entry for `cowork-obsidian` — including manually-repacked attempts or stale uploads
> 3. **Remove / delete it** (trash icon, "Uninstall", or "Remove")
> 4. 🔄 Quit Cowork fully → relaunch (clears the in-memory marketplace cache)
> 5. Proceed to the install steps below
>
> If this is your **first time** installing this plugin, skip this section.

This is the recommended install path for all Mac users. It bypasses Anthropic's open Cowork-on-macOS bugs (🚧 [#26951](https://github.com/anthropics/claude-code/issues/26951), 🚧 [#28125](https://github.com/anthropics/claude-code/issues/28125)) and works on every Cowork build that supports plugin uploads. Workaround confirmed by users in [#39400](https://github.com/anthropics/claude-code/issues/39400).

### ⏱️ 6 steps, ~30 seconds

1. **📦 Download** the latest **`cowork-obsidian-v0.5.0.zip`** (or `cowork-obsidian.zip`) from the [**Releases page → Assets**](https://github.com/AutomatedMarketer/cowork-obsidian/releases/latest). **Don't extract it.** Keep the file as a single `.zip`.
   > ⚠️ **Important:** download the zip from the **Releases page**, NOT the green **❌ Download ZIP** button at the top of the repo page. That button wraps the repo in an outer folder (`cowork-obsidian-main/`) which double-nests the plugin and breaks Cowork's validator.
2. **⚙️ Open Claude Cowork** (the middle tab in Claude Desktop) → click your name (top right) → **Settings**.
3. **🔌 Customize → Browse plugins** → look for the option to **upload a custom plugin file**.
   > 💡 Menu wording varies slightly between Cowork versions — look for "Upload", "Custom plugin", or "From file".
4. **📤 Drag in the zip.** Wait for confirmation.
5. **🚀 Open a brand new Cowork task** (skills load on session start, not retroactively).
6. Type `/onboard-second-brain` and follow the wizard.

> 💡 **Why this is the recommended Mac path:** Anthropic closed [#27196](https://github.com/anthropics/claude-code/issues/27196) ("All Anthropic plugins fail in Cowork on macOS") as **not planned** — they don't currently intend to fix the marketplace path on Mac. The zip-upload path is effectively the supported install method on macOS going forward.

### 🐛 Troubleshooting

| Symptom | What to do |
|---|---|
| 🤐 Upload appears to succeed but plugin doesn't update / still showing old version | You have a stale entry from a previous upload attempt. Remove the old entry first (see the "Already tried installing" callout at the top of this section). Cowork silently rejects same-name re-uploads. |
| 🔍 Can't find "Upload" / "Custom plugin" option | Look for "From file" / "Local plugin" / "Add manually". If genuinely absent, your Cowork version is older than the upload feature — 🔄 quit, update Claude Desktop, relaunch. |
| 📄 Upload rejects the file | Confirm the file extension is `.zip` (not `.plugin`). Re-download directly from the [Releases page](https://github.com/AutomatedMarketer/cowork-obsidian/releases/latest); don't rename. |
| 🚫 Plugin uploads but `/onboard-second-brain` does nothing | Open a **brand new** Cowork task. Skills load on session start, not retroactively. |
| 👻 Plugin disappears after restart | That's 🚧 [#38429](https://github.com/anthropics/claude-code/issues/38429) — Anthropic-side persistence bug. 🔄 Re-upload the zip after restart. |

### Windows

<details>
<summary><b>Windows install (click to expand)</b></summary>

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
```

Then in a fresh Cowork task:

```
/onboard-second-brain
```

</details>

### Linux

<details>
<summary><b>Linux install (click to expand)</b></summary>

Same as Windows:

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

</details>

---

## Use cases / case studies

Why VCInc cohort members and other solopreneurs / consultants set this up:

1. **Agency owners** — client deliverables on demand from indexed past work
2. **Coaches** — distilled client frameworks across 50+ conversations
3. **Course creators** — evergreen curriculum that compounds across cohorts
4. **Consultants** — research compound across years of engagements
5. **Writers / creators** — long-form ideas surfaced from years of brain dumps
6. **Multi-role operators** — work + business + health + personal in one indexed system

Full case studies (~150 words each, with exact `raw/wiki/output` flow per persona): [docs/use-cases.md](./cowork-obsidian/docs/use-cases.md)

The `/second-brain` skill itself can ask you which persona fits best and tailor its first-run suggestions to your role. Just run `/second-brain` with no arguments after onboarding.

---

## Coordination with `cowork-ai-os`

This plugin is **standalone** — you can use it without [`cowork-ai-os`](https://github.com/AutomatedMarketer/cowork-ai-os) installed.

- If `cowork-ai-os` is detected at v0.7.0+ → cowork-obsidian uses its existing `safe-zones.md` and integrates with `/morning-brief`, `/voice-writer` (when those eventually read `vaults.md`).
- If `cowork-ai-os` is **not** detected → cowork-obsidian ships a self-contained minimal `safe-zones.md` and works on its own.

Cohort students using both: install `cowork-ai-os` first (Project 01: Core Setup), then `cowork-obsidian` (Project 04: Second Brain).

---

## Curriculum

Project 04 maps 1:1 to a curriculum module at [vcinc.com](https://vcinc.com) for VCI cohort members. Each phase of the install corresponds to a lesson in the cohort.

---

## License

[MIT](./LICENSE) — yours forever.

## Built by

[Nuno Tavares](https://nunomtavares.com) — Newsletter: [automatedmarketer.net](https://automatedmarketer.net) · YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
