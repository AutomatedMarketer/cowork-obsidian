# cowork-obsidian v0.1.0 — initial release

**Project 04 ships as its own plugin.** A local-first Obsidian second brain wired into Claude Cowork — **no MCP, no API keys, no Local REST API plugin, no VS Code, no git.** Just Obsidian + your vault folder + Cowork's existing filesystem access, scoped by a safe-zones carve-out.

---

## What's new

This is the first release. Everything is new.

### A new install walkthrough: `/onboard-second-brain`

Six phases, ~70 minutes, pause-friendly.

| Phase | What it does |
|---|---|
| 0 | Welcome + verify cowork-ai-os v0.7.0+ + verify Project 01 complete + scaffold projects folder |
| 1 | Interview to capture `vaults.md` — 1 vault for beginners, up to 3 for power users, max 4 life areas per vault |
| 2 | Walk through installing Obsidian (download from obsidian.md, launch). Auto-detect existing installs and skip. |
| 3 | Scaffold first vault on a local drive (forbid cloud-sync paths). Create life-area folders × `raw/`/`wiki/`/`output/`. Copy `about-me/` from workspace into vault. Seed 2–3 raw notes. |
| 4 | Add vault path to `safe-zones.md` as a `/second-brain`-scoped carve-out. Test read + write. **No MCP, no API key.** |
| 5 | Run `build` for the first time. Explain `update` and `health-check`. Establish weekly rhythm. Optionally write `My Vault System.md`. |

### A new operational skill: `/second-brain`

Three canonical commands plus passive read mode:

- **`build [life-area]`** — first-pass wiki construction from raw notes. One topic per page, cross-linked via `[[wikilinks]]`.
- **`update [life-area]`** — fold new raw notes into existing wiki. Detects contradictions and surfaces them rather than silently overwriting.
- **`health-check [life-area]`** — flag contradictions, missing topics, stale pages, orphans, output→wiki gaps. Reports only; never auto-fixes.
- **Passive read** — when you ask a question (e.g. *"what does my second brain say about pricing?"*), the skill searches `wiki/` and `output/` across all configured vaults and surfaces relevant pages with citations.
- **`fold-back [output filename]`** *(optional)* — absorb a finished output back into the wiki.

### Five hard rules baked into `/second-brain`

> "I never write to `raw/`. Raw is the user's writing space."

> "I never delete a wiki page. If stale or contradicted, I update it or flag it — never remove."

> "I only operate on configured vault paths from `vaults.md`."

> "I only operate on the three subfolders `raw/`, `wiki/`, `output/` inside each life-area folder. Vault root, `about-me/`, and `.obsidian/` are off-limits unless explicitly directed."

> "If the user says STOP at any time, I halt immediately."

Verbatim. Never paraphrased. These apply to every command.

---

## Architectural decisions

### Why no MCP for v0.1.0

Cowork already has filesystem access. If your vault path is allow-listed in `cowork-ai-os`'s `safe-zones.md` as a `/second-brain`-scoped carve-out, Cowork can read and write `.md` files in your vault directly. **MCP is unnecessary for the basic read/write that the three prompts need.**

MCP becomes useful for richer features (semantic search across the vault, link-graph queries, full-text indexing). Those are deferred to v0.3.0+. When we ship MCP support, we'll fork [bitbonsai/mcpvault](https://github.com/bitbonsai/mcp-obsidian) — actively maintained, direct file access (no Local REST API plugin needed).

### Why no Local REST API plugin

The most common Obsidian-MCP integration path requires users to install Obsidian → install the Local REST API community plugin → generate an API key → paste it into MCP config. That's 4 steps that drop conversion for non-developers. We skip the entire path. Your vault is plain markdown files; we read them with the filesystem.

### Why no VS Code, no git

The cohort audience is non-developer entrepreneurs. VS Code and git both require a developer mental model. Power users can layer them on independently — that's not in the v0.1.0 onboarding.

### Why multi-vault from day one

Nuno (the author) runs separate vaults per context (one for cowork plugin work, one for business, one for personal). The pattern provides privacy and focus. Adding multi-vault support after the fact would require migrating users; building it in from v0.1.0 is cleaner.

---

## Coordination with `cowork-ai-os`

This plugin **depends on** `cowork-ai-os` v0.7.0+:
- Phase 0 verifies cowork-ai-os is installed and at the right version
- Uses cowork-ai-os's `safe-zones.md` mechanism for the carve-out

This plugin is **decoupled from** future cowork-ai-os behavior:
- Ships standalone — works without `/morning-brief`, `/tidy-downloads`, etc.
- Future cowork-ai-os v0.8.0 will detect `vaults.md` and let `/morning-brief` and `/voice-writer` optionally read from your vault. That cross-skill wiring is deferred.

If you don't have `/onboard-file-organization` already run, Phase 0 offers a "minimal safe-zones.md" option that creates just the second-brain carve-out without the full file-tidy onboarding.

---

## Install

### Mac (recommended path)

`/plugin update` doesn't reliably work on Mac due to Anthropic's open marketplace bugs ([#26951](https://github.com/anthropics/claude-code/issues/26951), [#28125](https://github.com/anthropics/claude-code/issues/28125)).

1. Download `cowork-obsidian.zip` from the assets below
2. Open Claude Desktop → click your name (top right) → **Settings**
3. **Customize** → **Browse plugins** → upload the zip
4. Open a fresh Cowork task → run `/onboard-second-brain`

### Windows

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

---

## Files in this release

```
+ .claude-plugin/marketplace.json
+ cowork-obsidian/.claude-plugin/plugin.json
+ cowork-obsidian/skills/second-brain/SKILL.md
+ cowork-obsidian/skills/onboard-second-brain/SKILL.md
+ cowork-obsidian/skills/onboard-second-brain/phases/00-welcome.md
+ cowork-obsidian/skills/onboard-second-brain/phases/01-plan-vaults.md
+ cowork-obsidian/skills/onboard-second-brain/phases/02-install-obsidian.md
+ cowork-obsidian/skills/onboard-second-brain/phases/03-scaffold-vault.md
+ cowork-obsidian/skills/onboard-second-brain/phases/04-wire-cowork-to-vault.md
+ cowork-obsidian/skills/onboard-second-brain/phases/05-three-prompts.md
+ cowork-obsidian/skills/onboard-second-brain/templates/state-second-brain.template.md
+ cowork-obsidian/skills/onboard-second-brain/templates/vaults.template.md
+ cowork-obsidian/skills/onboard-second-brain/templates/vault-folder-structure.template.md
+ cowork-obsidian/skills/onboard-second-brain/templates/prompts.template.md
+ cowork-obsidian/skills/onboard-second-brain/templates/memory.template.md
+ cowork-obsidian/skills/onboard-second-brain/samples/sample-vaults.md
+ build-release-zip.sh
+ CHANGELOG.md
+ README.md
+ LICENSE
+ .gitignore
```

---

## Curriculum

Project 04 — Second Brain in Obsidian — maps 1:1 to a module at [vcinc.ai](https://vcinc.ai). Each phase of the install corresponds to a cohort lesson.

---

## What's next

- **v0.2.0 (likely 2–4 weeks):** cross-skill wiring with `cowork-ai-os` v0.8.0 — `/morning-brief` reads `vault/<area>/raw/today.md`, `/voice-writer` pulls `Brand Voice.md` from the vault, `/tidy-downloads` ignores the vault path via the carve-out
- **v0.3.0+ (TBD):** optional MCP server for semantic search and link-graph queries (likely fork of [bitbonsai/mcpvault](https://github.com/bitbonsai/mcp-obsidian))
- **Power-user phases (TBD):** optional VS Code + git wiring for users who want a developer-style PKM workflow

---

## Built by

[Nuno Tavares](https://nunomtavares.com) for VCI cohort students and anyone who wants a knowledge base that compounds for years instead of a chatbot that forgets last Tuesday.

- Newsletter: [automatedmarketer.net](https://automatedmarketer.net)
- YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
- VCI cohort: [vcinc.ai](https://vcinc.ai)
