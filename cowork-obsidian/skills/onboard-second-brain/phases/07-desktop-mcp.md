# Phase 7 — Claude Desktop MCP (Optional)

**Goal:** Help the user wire their separate Claude Desktop chat app to read the same vault via an MCP server. **Only run this phase if the user explicitly wants Claude Desktop integration in addition to Cowork.** Cowork already has filesystem access — it does not need MCP.

**Time:** ~5–15 minutes depending on path chosen.

**Optional:** Skip if user only uses Cowork. Most users can finish onboarding here and never come back to Phase 7.

---

## Script — what to say (in voice)

> *"Phase 7 is for users who also use Claude Desktop — the standalone chat app you launch from your Dock or Start menu. It's a different product from Claude Cowork.*
>
> *Cowork already reads your vault directly via filesystem access — that's how Phases 4 and 5 work. We don't need anything else for Cowork.*
>
> *But Claude Desktop has no native filesystem access — it only sees outside files through MCP servers. If you want Claude Desktop to also read your vault, this phase walks you through that.*
>
> *Skip this phase if:*
> *- You don't use Claude Desktop, OR*
> *- You're fine using Cowork as the only Claude that sees your vault.*
>
> *Continue if:*
> *- You actively use Claude Desktop and want it to also read your vault for chat queries.*
>
> *Either way, your Cowork-vault wiring from Phase 4 is unaffected by this phase."*

If user skips → set `phase_7_desktop_mcp: skipped` in state and finish onboarding.
If user continues → MCP path picker (next section).

---

## Connect Claude to your vault (MCP wiring)

This phase wires Claude to your vault. Two paths. Pick A unless you have a reason not to.

**Ask the user this exact question:**

> Path A connects Claude to your vault in 1 minute and works for everything. Path B takes 3 more minutes and lets Claude see whichever note you currently have open — useful if you live inside Obsidian. You can always upgrade later. Pick A.

Default: A.

Capture the choice in state as `phase_7_mcp_path: A` or `phase_7_mcp_path: B`.

---

## The trap warning (read before EITHER path)

Both paths edit the same config file. This warning applies to both.

> *"Important warning before we touch any config files.*
>
> *If you Google 'how to install an MCP server for Claude Desktop on Mac,' most guides will tell you to edit one of these files:*
>
> *- `~/.claude/settings.json` — **DOES NOT WORK for Claude Desktop.** That file is for Claude Code (Cowork) only.*
> *- `~/.claude/mcp.json` — **DOES NOT WORK either.** Claude Desktop tries an OAuth flow when it sees this and fails for non-OAuth MCP servers.*
> *- `enabledMcpjsonServers` field in some settings file — **also doesn't work.**.*
>
> *The ONLY file Claude Desktop actually reads for MCP server config is:*
>
> *- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`*
> *- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json` (= `C:\Users\<u>\AppData\Roaming\Claude\claude_desktop_config.json`)*
>
> *Anything else is a dead end. We're going to edit that file specifically. **Quit Claude Desktop fully (Cmd+Q on Mac, right-click → quit on Windows) before saving the file.** If Desktop is running, it won't pick up the changes until restart, and the connection will silently fail."*

---

## Path A — Anthropic Filesystem MCP (default)

This is the recommended default. It's first-party (Anthropic-maintained), needs no Obsidian plugin, doesn't need Obsidian to be running, and works for every vault file. Trade-off: Claude won't know which note you currently have open in Obsidian — pick Path B if that matters.

### A.1 — Detect OS and locate Claude Desktop config

```
mac     → ~/Library/Application Support/Claude/claude_desktop_config.json
windows → %APPDATA%\Claude\claude_desktop_config.json
                (= C:\Users\<u>\AppData\Roaming\Claude\claude_desktop_config.json)
```

### A.2 — Verify Node.js is installed (Filesystem MCP needs it)

```
node --version
```

Expected: `v18.x` or later. If missing or older:

- **Mac:** `brew install node` (if Homebrew is installed) OR download from https://nodejs.org
- **Windows:** Download from https://nodejs.org and run the installer

After install, re-run `node --version` to confirm.

### A.3 — Quit Claude Desktop fully

**Mac:** Cmd+Q on the Claude Desktop window (or right-click the menu bar icon → Quit).
**Windows:** Right-click the system tray icon → Quit. Confirm via Task Manager that no Claude Desktop process remains.

If you skip this step, the next steps' edits won't take effect.

### A.4 — Back up the existing config (BEFORE any write)

If `claude_desktop_config.json` exists, back it up to `<config>.bak.<YYYY-MM-DD-HHMMSS>` before touching it. Keep the backup until verification passes — restore from it if anything goes sideways.

If the config file doesn't exist, scaffold a fresh one with `{"mcpServers": {}}`.

### A.5 — Build the merged config

1. Read [`../templates/filesystem-mcp-config.json.template`](../templates/filesystem-mcp-config.json.template).
2. Substitute `{{vault_root_absolute_path}}` with the user's vault path (read from `projects/second-brain/vaults.md` → `default_vault` → `path`). Quote the path properly for the OS — handle spaces, non-ASCII characters, and Windows backslashes (use forward slashes or properly-escaped backslashes inside the JSON string).
3. Merge the resulting `mcpServers.obsidian-vault` entry into the existing config object (do NOT replace other servers — if `mcpServers` already has entries like `gohighlevel` or `obsidian` from a prior install, just add `obsidian-vault` as a sibling).
4. Show the user the diff between backup and proposed new config in a code block. Ask permission. Save when approved.

### A.6 — Validate the JSON parses

Re-read the saved config file. Validate it parses as JSON.

If invalid → restore from the backup made in A.4, surface the parse error to the user, and stop. Do not leave the user with a broken config.

If valid → continue.

### A.7 — Open Claude Desktop and verify

Tell the user:

> *"Restart Claude Desktop now. When it's back up, type 'list 3 files in my vault' and tell me what Claude says."*

Wait for the user to report back.

If Claude Desktop returns 3 vault filenames → Path A success. Skip to Step 8 (state update).

If it doesn't work, run the 3-step recovery:

1. **Re-check config path.** Did we write to the right file for the OS? (Mac: `~/Library/Application Support/Claude/`, Windows: `%APPDATA%\Claude\`.)
2. **Re-validate JSON.** Re-read the file, confirm it still parses, confirm `mcpServers.obsidian-vault` is intact.
3. **Re-check vault path readability.** Does the vault folder exist at the path we wrote into the config? Can the user open it in File Explorer / Finder?

If still failing → restore the backup, tell the user Path A didn't take, and offer Path B as a fallback.

---

## Path B — iansinnott Obsidian Claude Code MCP (opt-in)

Pick this only if you want Claude Desktop to know which note you currently have open in Obsidian (it uses a WebSocket bridge from the Obsidian-side plugin). Requires Obsidian to be open whenever you want to use it from Claude Desktop.

### B.1 — Verify Node.js is installed

Same as A.2. If missing, install per the OS instructions there.

### B.2 — Install the Obsidian-side plugin

Open Obsidian. Settings → Community plugins → Browse → search for **"Claude Code MCP"** (author: iansinnott) → install + enable.

This plugin opens a WebSocket on port 22360 (avoids common dev-server conflicts). It exposes the vault to MCP clients.

### B.3 — Quit Claude Desktop fully

**Mac:** Cmd+Q on the Claude Desktop window (or right-click the menu bar icon → Quit).
**Windows:** Right-click the system tray icon → Quit. Confirm via Task Manager that no Claude Desktop process remains.

If you skip this step, the next step's edits won't take effect.

### B.4 — Back up the existing config (BEFORE any write)

If `claude_desktop_config.json` exists, back it up to `<config>.bak.<YYYY-MM-DD-HHMMSS>` before touching it.

### B.5 — Edit `claude_desktop_config.json`

#### Mac path

```
~/Library/Application Support/Claude/claude_desktop_config.json
```

The user can navigate there via Finder → Cmd+Shift+G (Go to folder) → paste the path.

#### Windows path

```
%APPDATA%\Claude\claude_desktop_config.json
```

The user can paste that into File Explorer's address bar.

#### What to add

If the file doesn't exist, create it with this content. If it exists, merge the `obsidian` entry into the existing `mcpServers` object:

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "@iansinnott/obsidian-claude-code-mcp-bridge"]
    }
  }
}
```

Save the file.

> **Note for users with existing MCP servers (e.g., GoHighLevel, or `obsidian-vault` from a prior Path A install):** If `mcpServers` already has entries, just add the `obsidian` key as a sibling. Don't replace the whole object.

### B.6 — Open Claude Desktop and verify

1. Open Claude Desktop fresh (it must have been fully quit in B.3).
2. In Claude Desktop: click your name (top right) → **Customize** → **Connectors**.
3. Scroll to the **Desktop** section.
4. You should see **obsidian** listed and connected.

If it shows as "Not connected" or "CUSTOM":
- Confirm Obsidian is open (the WebSocket needs the Obsidian plugin running)
- Confirm Node.js is installed and on PATH
- Re-quit and reopen Claude Desktop
- Check the JSON syntax — common errors: trailing comma, missing quote, wrong file path

If still failing → roll back: restore the backup from B.4, re-quit + reopen Claude Desktop. Path B setup didn't take. The Cowork-side wiring from Phase 4 is unaffected; you can still use Cowork normally. Offer Path A as a simpler fallback.

### B.7 — Test in Claude Desktop

Ask Claude Desktop something that requires reading your vault. Example:

> *"Read my notes about <topic from one of your wiki pages>. Quote the wiki page directly."*

Claude Desktop should respond with content from the wiki page, citing the file path.

If yes → Path B successful.
If Claude Desktop says it can't read your files → the connector isn't actually connected. Re-walk B.3–B.6.

---

## Step 8 — Update state and finish onboarding

Update `_aibos/state-second-brain.md`:
- `phase_7_desktop_mcp: enabled`
- `phase_7_mcp_path: <A | B>`
- `desktop_mcp_server: <@modelcontextprotocol/server-filesystem | iansinnott/obsidian-claude-code-mcp>`
- `desktop_mcp_config_backup: <path to .bak file>` (so future re-runs can find the rollback point)
- `Phase 7 (desktop mcp): completed at <ISO timestamp>`
- `second_brain_complete: true`
- `completed_at: <ISO timestamp>`

Append to `projects/second-brain/memory.md`:

```markdown
## <ISO timestamp> — Phase 7 complete: Claude Desktop MCP enabled
- Path: <A — Filesystem MCP | B — iansinnott bridge>
- Server: <server name>
- Config file: <claude_desktop_config.json path>
- Config backup: <.bak path>
- Desktop verification: <"3 files listed via Filesystem MCP" | "Connectors panel shows obsidian connected">
```

Tell the user:

> *"Phase 7 done. Both Claude Cowork AND Claude Desktop can now read your vault.*
>
> *Cowork uses direct filesystem access (same as Phases 0–5).*
> *Desktop uses <Path A — the Anthropic Filesystem MCP, reads vault files directly | Path B — the iansinnott WebSocket bridge on port 22360, requires Obsidian to be open>.*
> *They don't conflict. Edits in either tool show up in both via the underlying vault files.*
>
> *cowork-obsidian install complete. Run `/second-brain build [life-area]` any time. Run `/onboard-second-brain` again to add another vault or revisit any phase."*

---

## Verification before calling done

**Path A:**
- Claude Desktop returns vault filenames when asked to list files
- `claude_desktop_config.json` parses as valid JSON
- `mcpServers.obsidian-vault` entry points at the user's actual vault path
- Backup file exists at the path recorded in state

**Path B:**
- Claude Desktop's Connectors panel shows `obsidian` as connected
- A test query in Claude Desktop returns vault content
- Backup file exists at the path recorded in state

**Both:**
- State file shows `phase_7_desktop_mcp: enabled`, `phase_7_mcp_path: <A | B>`, `second_brain_complete: true`

---

## Alternative MCP servers (advanced users only)

If neither Path A nor Path B fits, alternatives:

- [`MarkusPfundstein/mcp-obsidian`](https://github.com/MarkusPfundstein/mcp-obsidian) — requires the Obsidian Local REST API community plugin + an API key in env vars. More moving parts. Not recommended for newcomers.
- [`administrativetrick/obsidian-mcp`](https://github.com/administrativetrick/obsidian-mcp) — direct filesystem access, doesn't require Obsidian to be running. Different access model.
- [`marcelmarais/obsidian-mcp-server`](https://mcpservers.org/servers/marcelmarais/obsidian-mcp-server) — lightweight standalone, direct filesystem.

Each has different config requirements; consult that server's README. The Mac trap warning (`claude_desktop_config.json` is the only file Claude Desktop reads) applies to all of them.

---

## What this phase never does

- Edit `~/.claude/settings.json` or `~/.claude/mcp.json` for Claude Desktop (those are wrong — see the trap warning)
- Recommend pulling the user's vault into the cloud for Desktop to read remotely (Desktop reads via local MCP server, not cloud)
- Force Phase 7 on users who only use Cowork (it's optional — most users skip it)
- Store any credentials or API keys in plain text in the config file (none of the recommended servers require credentials)
- Modify Cowork's filesystem wiring from Phase 4 (Phase 7 is purely additive)
- Overwrite an existing `claude_desktop_config.json` without first backing it up
- Replace existing `mcpServers` entries (always merge, never clobber)
