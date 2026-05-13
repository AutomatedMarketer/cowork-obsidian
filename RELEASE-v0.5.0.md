# v0.5.0 — Memory Tiers and Filesystem MCP Default

## The big idea

In April 2026, Andrej Karpathy published a gist titled [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) that crystallized something the field had been circling: **a markdown vault is the cheapest, most durable long-term memory your AI agent will ever own.** Plain text. No vendor lock-in. Every Claude model can read it natively.

> "Obsidian is the IDE, the LLM is the programmer, the wiki is the codebase. You rarely ever write or edit the wiki manually." — Karpathy, April 4, 2026

cowork-obsidian v0.5.0 is the install + scaffold + wiring that makes this practical for Claude Cowork users. This release flips the architecture: instead of "a plugin that helps you set up Obsidian," it's "the plugin that gives Cowork a long-term memory."

## What's new

- **Four-tier memory model** baked into the bundled scaffold. Cowork loads ~800 tokens always (your identity + the vault hot cache) and only reaches for full wiki pages when a query specifically needs them. A typical vault question touches ~3,500 tokens of context, not 50,000.
- **Filesystem MCP is now the default wiring path.** Anthropic's official `@modelcontextprotocol/server-filesystem`, one minute of setup, no extra Obsidian plugins. The iansinnott Claude Code MCP is still here as the opt-in upgrade for power users who want Claude to see the note they currently have open.
- **Vault-path safety check** that refuses any path inside iCloud, OneDrive, Dropbox, or Google Drive — and offers to migrate an existing in-cloud vault to a safe local folder. Cloud sync corrupts Obsidian vaults; v0.5 makes that impossible to do by accident.
- **`/second-brain refresh-hot`** — fifth operation. Monthly, it regenerates the `wiki/hot.md` cache from the current wiki state so the always-loaded memory stays accurate as your wiki grows.
- **`/open-vault` got smarter.** It reads the hot cache + wiki index at session start, and tells you "you have N new raw notes since your last UPDATE — want me to run it?" The willpower tax is gone.
- **All SOPs rewritten at 3rd-4th grade reading level.** Every lesson now leads with the *why* (Karpathy citation), opens with the four-tier memory model, and treats the install steps as a wizard concern (not student concern).

## Install / upgrade

New install:

```
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

Upgrading from v0.4:

```
/plugin update cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

The wizard is resumable via `state.md` and skips phases you've already completed. Existing vaults stay untouched; only the MCP wiring updates.

## Breaking changes

**None.** v0.5 is additive. Path B (iansinnott MCP) keeps working for v0.4 users who were already on it. The phase numbers are preserved, so any in-flight `state.md` resumption continues to work.

## What's next

- **v0.6 — the compounding loop.** Boris Cherny's "anytime Claude does something wrong, write it down" pattern, automated. A `Stop`-hook that pulls lessons out of every session and folds them into the vault.
- **v0.7 — custom Cowork MCP server.** Once we've watched 20+ students hit specific friction, we ship `wiki_get`, `wikilink_resolve`, and `vault_search(quick|standard|deep)` as first-class MCP tools.

## Credits

Built by Nuno Tavares of Automated Marketer. Karpathy framing borrowed gratefully from the [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Cherny pattern from [howborisusesclaudecode.com](https://howborisusesclaudecode.com).
