# cowork-obsidian v0.3.0

**Date:** 2026-05-09
**Tag:** `v0.3.0`
**Builds on:** v0.2.0 (rename + Phase 0.5 + 3 vault modes + sync + Desktop MCP)

---

## TL;DR

v0.3.0 makes the plugin **app-like**. The user installs it once and gets **three Cowork commands** that cover the whole flow:

- `/onboard-second-brain` — one-time setup
- `/open-vault` *(NEW)* — daily-driver session starter
- `/second-brain` — operational skill (build / update / health-check / fold-back)

Plus the cohort SOPs are rewritten to **stop teaching the manual paste-the-prompt approach** that was a v0.1.0 holdover and **start teaching the plugin's actual skills.** Beginner-friendly. Less duplicate effort.

---

## What's new

### `/open-vault` skill — the daily-driver

The user's most-asked-for feature: *"a command every time they want to open up a second brain so it will initiate the vault."*

Now when a cohort member sits down to work on their second brain, they run **one command**:

```
/open-vault
```

(Or `open my vault`, `let me work on my second brain`, `start second brain session` — natural-language triggers.)

The skill:
1. Loads `vaults.md` and picks the right vault (asks if multiple).
2. Validates the vault on disk (path exists, `.obsidian/` present).
3. Reports vault status — mode, last activity, last build / update / health-check, count of new raw notes since last update.
4. **Launches Obsidian visually** via the `obsidian://open?vault=<name>` URL scheme on Mac/Windows/Linux. Falls back to opening the vault folder in the OS file manager if the URL scheme isn't registered.
5. Offers a next-step menu: build / update / health-check / fold-back / just-browse.
6. Logs the session open to `projects/second-brain/memory.md`.

This is the command that turns a setup into a habit. Run it once a day — the system tells you what's pending.

### Natural-language triggers across all 3 skills

Beginners don't memorize slash commands on day one. v0.3.0 expands the trigger phrase lists so common natural language fires the right skill:

| Skill | New trigger phrases |
|---|---|
| `/onboard-second-brain` | "start onboarding", "start second brain", "begin second brain setup", "start the obsidian onboarding" |
| `/open-vault` *(all triggers are new)* | "open my vault", "open my second brain", "let me work on my second brain", "start second brain session", "start my vault", "begin a second brain session" |
| `/second-brain` | "refresh my wiki", "audit my second brain", "fold output into wiki", "run my second brain", "process my raw notes" |

### SOP-C04-05 — Use Cases & Daily Workflow (NEW)

The 5th cohort SOP. Read-only "now what?" guide that answers the question every new student asks: *"I set this up — what do I actually do with it tomorrow morning?"*

Includes:

- **6 case studies** by role (coach, agency owner, course creator, consultant, writer, multi-business operator) with exact daily / weekly / monthly cadence per role.
- **References to the second-brain thinkers** who inform the model: Julie Chenell (the human / robot / human + robot framing), Tiago Forte (PARA, *Building a Second Brain*), Andy Matuschak (atomic / evergreen notes), and the Obsidian community.
- **A workflow table** with exact commands at each cadence (daily / weekly / monthly / quarterly / per-session / first-run).
- **A real daily session walkthrough** — a Tuesday morning sitting-down-to-work scene with the actual `/open-vault` → `/second-brain update` flow.
- **7 first-month pitfalls** (hand-editing the wiki, skipping `/open-vault`, bulk-importing notes, etc.) with how to avoid each.

---

## What changed

### SOP-C04-02 — rewritten skill-first

The v0.1.0 SOP-C04-02 had students paste a 200-word prompt manually into a New Task to run build → update → health-check end-to-end. That made sense **before the plugin existed**.

v0.3.0 SOP-C04-02 just uses the plugin's `/second-brain` skill:

```
/second-brain build coaching
/second-brain update coaching
/second-brain health-check coaching
```

The skill knows the prompts. The student doesn't paste. Lesson time dropped from ~30 min to ~15 min. The full prompt templates are now in an appendix — for reference / debugging / reusing in other AI tools, not for daily use.

### SOP-C04-04 — dropped the custom-skill build

The v0.1.0 SOP-C04-03 had students BUILD a custom "Vault Wiki Workflow" skill via Skill Creator. That made sense before the plugin shipped its own operational skill.

v0.3.0 drops that lesson entirely. The plugin already has `/second-brain` (operational) and now `/open-vault` (daily-driver). Building another skill that does the same thing is duplicate work and an unnecessary surface to maintain.

The new SOP-C04-04 focuses on:
- Calendar reminders for the weekly / monthly / quarterly cadence
- Home indexes (`_index.md`) for any wiki past 5 pages
- A golden output saved as a reference deliverable
- The `My Vault System.md` file (deferred from Lesson 1)
- Establishing `/open-vault` as the daily-startup pattern

Lesson time dropped from ~30 min to ~20 min.

### SOP-C04-00 (Overview) and SOP-C04-01 (Connect or Create)

Updated to mention all 3 skills explicitly. The 3-command suite is the centerpiece of the project framing now.

### Plugin metadata + README

- `plugin.json` and `marketplace.json` descriptions updated to highlight the 3 skills.
- README has a new top-of-file section: *"The three commands you'll use forever"* — table of the 3 skills with when-to-use guidance.
- Badges bumped to v0.3.0.

---

## Migration from v0.2.0

Painless. No breaking changes:

- **Plugin install command unchanged:** `/plugin install cowork-obsidian@cowork-obsidian`.
- **Existing slash commands preserved:** `/onboard-second-brain` and `/second-brain` work exactly as before, just with expanded natural-language triggers.
- **`/open-vault` skill auto-installs** when the plugin updates. No user action needed.
- **`vaults.md` and state file formats unchanged.** No migration required.
- **Old SOP filenames preserved.** `SOP-C04-02.md` and `SOP-C04-04.md` were rewritten in place; `SOP-C04-05.md` added.

If a v0.2.0 user wants to refresh: re-download `cowork-obsidian.zip` from the v0.3.0 release and re-upload via Settings → Customize → Browse plugins (Mac) or `/plugin update cowork-obsidian` (Windows).

---

## Install

### Mac

1. Download `cowork-obsidian.zip` from the [latest release](https://github.com/AutomatedMarketer/cowork-obsidian/releases/latest)
2. Open Claude Desktop → **Settings** → **Customize** → **Browse plugins** → upload the zip
3. Open a fresh Cowork task → run `/onboard-second-brain` (or `start onboarding`)

### Windows

```
/plugin marketplace add AutomatedMarketer/cowork-obsidian
/plugin install cowork-obsidian@cowork-obsidian
/onboard-second-brain
```

After onboarding, your daily command is:

```
/open-vault
```

---

## What stays the same

- 6-phase backbone (Phase 0 → Phase 0.5 → Phase 1/2/3 if scaffolding → Phase 4) + optional Phase 6 (sync) + optional Phase 7 (Desktop MCP).
- Three vault modes: `external`, `overlay`, `scaffold`.
- Soft `cowork-ai-os` prerequisite (works standalone if not installed).
- The hard rules in `/second-brain` SKILL.md — never delete a wiki page, never write to `raw/`, mode-aware writes, halt on `STOP`.
- Cloud-sync forbidden list for the vault folder location.
- Multi-vault support via `vaults.md`.
- The "second brain" *concept* and the Julie Chenell framing: *"there's a human in raw, a robot in wiki, and a human AND a robot in output."*

---

## What's still deliberately not in scope

- Native Obsidian plugin authoring (cowork-obsidian stays a Cowork plugin only)
- Standalone CLI installer (`npx cowork-obsidian init`) — defer to v0.4.0 if demand surfaces
- Vault encryption beyond Obsidian Sync's built-in E2EE
- Bulk import from Notion / Evernote / Apple Notes
- Multi-language SOPs / README — English only

---

## License

[MIT](./LICENSE) — yours forever.

## Built by

[Nuno Tavares](https://nunomtavares.com) — Newsletter: [automatedmarketer.net](https://automatedmarketer.net) · YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
