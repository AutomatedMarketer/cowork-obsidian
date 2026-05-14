# cowork-obsidian Architecture

> Internal architecture doc. Voice: technical. Audience: contributors and integrators.

## v0.5.0 — Memory Tiers and Filesystem MCP Default

This release moves cowork-obsidian from "a plugin that scaffolds an Obsidian vault" to "the plugin that gives Cowork a long-term memory." The major architectural changes:

### The four-tier memory model

| Tier | What | Where | Loaded when | ~Token cost |
|---|---|---|---|---|
| 1 | Identity | `about-me/` (cowork-ai-os workspace) | Skills that need it (cowork-ai-os v0.9 hot/cold split) | 300 |
| 2 | Vault hot cache | `<vault>/wiki/hot.md` | Start of any vault session via the skill prompt | 500 |
| 3 | Vault index | `<vault>/<area>/wiki/_index.md` per area (built by BUILD/UPDATE at 5+ pages) | When a query needs to pick a topic | 1,000 |
| 4 | Full wiki pages | `<vault>/<area>/wiki/<topic>.md` | Only when explicitly named or matched | 100–300 each |

**Rationale:** A typical vault query touches ~3,500 tokens. Always-loading the full vault would be ~50,000 tokens per prompt — unworkable for `cowork-ai-os` v0.9 which spent significant effort getting cold-start under 3,000 tokens. The four tiers give Claude awareness of the vault without pre-loading it.

### MCP path decision rationale

Two paths offered, one default. Detailed comparison from research (May 2026):

- **Path A (default): Anthropic Filesystem MCP** (`@modelcontextprotocol/server-filesystem`). Install difficulty 1/5. No Obsidian plugins required. Identical behavior on Mac/Win/Linux. Trade-off: no wikilink/frontmatter awareness — but skills can teach Claude vault conventions in prompts, which matches the cowork thesis.
- **Path B (opt-in): `iansinnott/obsidian-claude-code-mcp`**. Install difficulty 2/5 (one Obsidian community plugin). Gains current-open-note awareness via `/ide`. The same MCP wired by cowork-obsidian Phase 7 since v0.4.

**Skipped (with rationale):**
- `MarkusPfundstein/mcp-obsidian` — last push 2025-06-28, brittle on Windows (uvx path resolution), 11 months stale.
- `cyanheads/obsidian-mcp-server` — most actively maintained (516 stars, pushed 2026-05-11), 14 surgical-edit tools including frontmatter/tag ops, but install difficulty 4/5 (Bun + Local REST API + env vars). Not beginner-friendly.
- `jacksteamdev/obsidian-mcp-tools` (807 stars) — author has declared unmaintained.
- `aaronsb/obsidian-mcp-plugin` (305 stars, pushed 2026-05-11) — promising, includes Bases + Graph + Dataview tools, but young. Re-evaluate for v0.6.
- **Custom Cowork MCP server** — premature per research. Build in v0.7+ once 20+ students have hit specific friction.

### Vault-path safety architecture

Cloud-sync detection runs in two phases (both phases that decide a vault path):
- Phase 0.5 (`connect-or-create`): catches existing vaults already in cloud sync
- Phase 3 (`scaffold-vault`): catches new-vault path proposals in cloud sync

The detector lives at `skills/onboard-second-brain/checks/cloud-sync-detector.md` as a checklist Claude follows. See [`skills/onboard-second-brain/checks/cloud-sync-detector.md`](../skills/onboard-second-brain/checks/cloud-sync-detector.md) for the 3-branch logic (scaffold-new / connect-existing / overlay).

After Phase 1 completes, the check re-runs on the FINAL path as a verify step — catches any sneaky mid-flow override.

### Resumability via `state.md`

Both before and after v0.5, the wizard writes phase state to `_aibos/state-second-brain.md` after each phase completes. Resumption: re-running `/onboard-second-brain` reads `state.md` and resumes from the next un-completed phase. v0.5 phase numbering preserved (0, 0.5, 1–7) precisely so resumption continues to work for users mid-flight at upgrade time.

### Manifest version drift fix

Pre-v0.5 the repo shipped releases as v0.4.0 while `plugin.json` and `marketplace.json` still said `0.3.0` — a long-standing drift. v0.5.0 reconciles all version references (plugin.json, marketplace.json, README badge, SKILL.md frontmatter, build script) to `0.5.0`.

## Deferred to later versions

- **Stop-hook auto-extract-to-vault loop** (Cherny compounding pattern) → v0.6. Some students may find auto-writes-on-stop too magical/scary on first contact; ship after read-side hot.md pattern is proven.
- **Custom Cowork MCP server** (`wiki_get`, `wikilink_resolve`, `vault_search` modes) → v0.7. Build only after observing student friction.
- **Growth-tier skill bundling** (`/wiki-query`, `/save`, `/weekly-review`) → v0.6 or sibling plugin. Currently lives as personal skills in Nuno's vault, not as installable plugins.

## See also

- [Spec: 2026-05-12 cowork-obsidian v0.5 design](../../../Claude%20Code%20-%20Second%20Brain/Claude%20Co-Work/docs/superpowers/specs/2026-05-12-cowork-obsidian-v0.5-design.md)
- [Implementation plan: 2026-05-13](../../../Claude%20Code%20-%20Second%20Brain/Claude%20Co-Work/docs/superpowers/plans/2026-05-13-cowork-obsidian-v0.5-implementation.md)
- [Cloud-sync detector checklist](../skills/onboard-second-brain/checks/cloud-sync-detector.md)
