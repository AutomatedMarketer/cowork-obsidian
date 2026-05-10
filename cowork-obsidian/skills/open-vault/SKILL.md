---
name: open-vault
description: Daily-driver command. Launches Obsidian to the configured vault AND preps the Cowork session for second-brain work. Triggers on "/open-vault", "open my vault", "open my second brain", "let me work on my second brain", "start second brain session", "start my vault", "begin a second brain session". Reads `projects/second-brain/vaults.md`, picks the default vault (or asks if multiple), reports vault status (mode, last activity, new raw notes since last update), launches Obsidian via the `obsidian://open?vault=<name>` URL scheme, and offers a next-step menu (build / update / health-check / capture / browse). Use this every time the user starts working on their second brain.
---

# Open Vault — Daily-Driver Session Starter

You are the user's "I'm sitting down to work on my second brain" command. By the end of running this skill, the user has Obsidian open to the right vault, Cowork loaded with vault context, and a clear sense of what to do next.

This is the command they should run **every time** they want to start a second-brain session. Beginners especially benefit — they don't need to remember `/second-brain build coaching` or navigate Obsidian's Open Vault menu. They run `/open-vault` and the system prepares everything.

---

## Hard rules — VERBATIM

> "I never modify the vault structure when running this skill. I only read state and report what I find."

> "I never run `build`, `update`, `health-check`, or `fold-back` automatically. I report status and offer them as next steps. The user picks."

> "I never launch Obsidian to a path that's not in `vaults.md`. If a path is requested but not registered, I refuse and point the user to `/onboard-second-brain`."

> "I touch `.obsidian/` only to read it for vault validation. Never write."

> "If the user says `STOP`, I halt immediately and report what I had done so far."

These rules apply every run. They never relax.

---

## Step 1 — Load vault config

Read `projects/second-brain/vaults.md`.

- **If missing or empty** → STOP. Tell the user: *"`vaults.md` is missing or empty. Run `/onboard-second-brain` first to set up your vault. I'll be ready after that."*
- Parse the list of vaults. Each entry has: name, path, mode (`external`/`overlay`/`scaffold`), optional `overlay_root`, life areas, purpose.
- Note the `default_vault`.

## Step 2 — Pick the vault to open

- **If the user passed an argument** (e.g., `/open-vault business`) → match against vault names. If no match, list configured vaults and ask.
- **If no argument and only one vault is configured** → use it.
- **If no argument and multiple vaults are configured** → ask which one. Show name + purpose for each:

  > *"You have these vaults configured:*
  >
  > *1. **business** — coaching practice & client deliverables (`scaffold` mode)*
  > *2. **personal** — daily journal + book notes (`scaffold` mode)*
  > *3. **research** — connected to existing PARA vault (`external` mode)*
  >
  > *Which one are you working on today? (1, 2, 3, or name)."*

## Step 3 — Validate the vault on disk

For the picked vault:

- Verify the path exists on disk. If not → STOP, tell the user the path is missing, ask them to fix `vaults.md` or restore the folder.
- Verify the path contains a `.obsidian/` folder (proves it's a real Obsidian vault). If not → warn the user but proceed; some users have folders that aren't yet registered with Obsidian.
- For `overlay` mode: verify `overlay_root` exists and contains `raw/`, `wiki/`, `output/`. If missing → suggest re-running `/onboard-second-brain` to repair the overlay.
- For `scaffold` mode: verify at least one life-area folder exists with `raw/`, `wiki/`, `output/`. If missing → suggest re-running `/onboard-second-brain` Phase 3.

## Step 4 — Report vault status

Read recent state and produce a friendly status report:

```
You're in: <vault-name> (<mode> mode)
Path: <absolute path>

Last vault activity: <date>  (read from projects/second-brain/memory.md)
Last `build` ran on: <area> at <date>  (or "never")
Last `update` ran on: <area> at <date>  (or "never")
New raw notes since last update: <count>  (count files in raw/ newer than last update timestamp)

Vault contents:
  about-me/        <count> files          (skip if external mode)
  <area-1>/raw/    <count> raw notes — <count> new since last update
  <area-1>/wiki/   <count> pages
  <area-1>/output/ <count> deliverables
  ... (one row per life area / overlay sub-folder; for external mode just summarize "you have X folders, last touched <date>")
```

Keep this report under 25 lines. It's a status check, not a deep audit. If the user wants deep, they run `/second-brain health-check`.

## Step 5 — Launch Obsidian visually

After reporting status, launch Obsidian to this vault.

### Mac

```bash
open "obsidian://open?vault=<URL-encoded vault basename>"
```

### Windows

```cmd
cmd /c start "" "obsidian://open?vault=<URL-encoded vault basename>"
```

### Linux

```bash
xdg-open "obsidian://open?vault=<URL-encoded vault basename>"
```

The `obsidian://` URL scheme works if the vault has been opened in Obsidian at least once before (it gets registered in `obsidian.json`). If the URL scheme fails, fall back to opening the vault folder in the OS file manager:

- Mac: `open <vault-path>`
- Windows: `cmd /c start "" "<vault-path>"`
- Linux: `xdg-open <vault-path>`

…and tell the user: *"Obsidian's URL scheme didn't catch — I opened the vault folder in your file manager instead. Drag the folder onto Obsidian to open it as a vault."*

If neither works (e.g., headless environment), report: *"Couldn't launch a window from this environment. Open Obsidian yourself and select the vault `<name>`. I've still loaded the Cowork session context — you can keep using me."*

Don't STOP on launch failure. The Cowork session prep still has value.

## Step 6 — Offer the next-step menu

After the launch attempt, ask the user what they want to do:

> *"Obsidian's coming up with **<vault>** open. What now?*
>
> *1. **Build** a fresh wiki for an area → `/second-brain build [area]`*
> *2. **Update** an existing wiki with new raw notes → `/second-brain update [area]`*
> *3. **Health check** an area → `/second-brain health-check [area]`*
> *4. **Fold back** outputs into the wiki → `/second-brain fold-back [area]`*
> *5. **Just browse** — I'll wait. Tell me when you want to do something.*
>
> *Or describe what you want to do and I'll figure out the right command. (E.g., 'capture a quick note about today's client call', 'find what my wiki says about pricing objections', 'show me my newest wiki pages')."*

If the user picks 1–4, hand off to `/second-brain` with the chosen operation.
If the user picks 5 (browse), pause and stay ready.
If the user describes something in natural language, route to the appropriate command or operation.

## Step 7 — Log the open

Append to `projects/second-brain/memory.md`:

```markdown
## <ISO timestamp> — Session opened on <vault-name>
- Mode: <mode>
- Status reported: <count> raw notes new since last update
- Obsidian launched: <success | fallback | failed>
- User chose: <operation | browsing>
```

Light log — just for the future "when did I last touch this?" audit.

---

## What this skill never does

- Modify the vault structure (it's a status + launch command, not an editor)
- Run `build`, `update`, `health-check`, or `fold-back` automatically (the user picks)
- Open a vault that isn't in `vaults.md` (refuse and point at `/onboard-second-brain`)
- Touch `.obsidian/` (read-only validation only)
- Persist any state outside `memory.md`

---

## When the skill should refuse

Refuse with a clear message and DO NOT proceed if any of these are true:

- `vaults.md` is missing, unreadable, or empty
- Target vault path is not in `vaults.md`
- Target vault path doesn't exist on disk

For each refusal, say *why* in one sentence and point at `/onboard-second-brain`.

---

## Beginner-friendly notes

- This skill should feel like opening an app. The user types one command and the system gets out of their way.
- Status report should fit on one screen. Brevity is a feature.
- Don't lecture about modes / structure / the underlying model. They learned that in onboarding.
- If something fails (Obsidian not launching, vault path missing), explain what they can do in plain language. No stack traces. No jargon.
- Default to action. If the user has only one vault → just open it. If they have one obvious "current focus" → suggest it. Reduce decisions where you can.
