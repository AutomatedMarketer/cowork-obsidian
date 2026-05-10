# Phase 7 — Claude Desktop MCP (Optional)

**Goal:** Help the user wire their separate Claude Desktop chat app to read the same vault via an Obsidian MCP server. **Only run this phase if the user explicitly wants Claude Desktop integration in addition to Cowork.** Cowork already has filesystem access — it does not need MCP.

**Time:** ~10 minutes.

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
If user continues → Step 1.

---

## Step 1 — The trap warning (lead with this)

This phase exists partly because most online guides give the wrong instructions for Claude Desktop on Mac. Tell the user verbatim:

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

## Step 2 — Verify Node.js is installed (the chosen MCP server needs it)

We'll use [`iansinnott/obsidian-claude-code-mcp`](https://github.com/iansinnott/obsidian-claude-code-mcp) as the server. It uses WebSocket auto-discovery and requires Node.js (npx).

Check Node.js:

```
node --version
```

Expected: `v18.x` or later. If missing or older:

- **Mac:** `brew install node` (if Homebrew is installed) OR download from https://nodejs.org
- **Windows:** Download from https://nodejs.org and run the installer

After install, re-run `node --version` to confirm.

---

## Step 3 — Install the Obsidian-side plugin

Open Obsidian. Settings → Community plugins → Browse → search for **"Claude Code MCP"** (author: iansinnott) → install + enable.

This plugin opens a WebSocket on port 22360 (avoids common dev-server conflicts). It exposes the vault to MCP clients.

---

## Step 4 — Quit Claude Desktop fully

**Mac:** Cmd+Q on the Claude Desktop window (or right-click the menu bar icon → Quit).
**Windows:** Right-click the system tray icon → Quit. Confirm via Task Manager that no Claude Desktop process remains.

If you skip this step, the next step's edits won't take effect.

---

## Step 5 — Edit `claude_desktop_config.json`

### Mac path

```
~/Library/Application Support/Claude/claude_desktop_config.json
```

The user can navigate there via Finder → Cmd+Shift+G (Go to folder) → paste the path.

### Windows path

```
%APPDATA%\Claude\claude_desktop_config.json
```

The user can paste that into File Explorer's address bar.

### What to add

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

> **Note for users with existing MCP servers (e.g., GoHighLevel):** If `mcpServers` already has entries, just add the `obsidian` key as a sibling. Don't replace the whole object.

---

## Step 6 — Open Claude Desktop and verify

1. Open Claude Desktop fresh (it must have been fully quit in Step 4).
2. In Claude Desktop: click your name (top right) → **Customize** → **Connectors**.
3. Scroll to the **Desktop** section.
4. You should see **obsidian** listed and connected ✅.

If it shows as "Not connected" or "CUSTOM":
- Confirm Obsidian is open (the WebSocket needs the Obsidian plugin running)
- Confirm Node.js is installed and on PATH
- Re-quit and reopen Claude Desktop
- Check the JSON syntax — common errors: trailing comma, missing quote, wrong file path

If still failing → roll back: remove the `obsidian` entry from `claude_desktop_config.json`, re-quit + reopen Claude Desktop. Phase 7 setup didn't take. The Cowork-side wiring from Phase 4 is unaffected; you can still use Cowork normally.

---

## Step 7 — Test in Claude Desktop

Ask Claude Desktop something that requires reading your vault. Example:

> *"Read my notes about <topic from one of your wiki pages>. Quote the wiki page directly."*

Claude Desktop should respond with content from the wiki page, citing the file path.

If yes → Phase 7 successful.
If Claude Desktop says it can't read your files → the connector isn't actually connected. Re-walk Steps 4–6.

---

## Step 8 — Update state and finish onboarding

Update `_aibos/state-second-brain.md`:
- `phase_7_desktop_mcp: enabled`
- `desktop_mcp_server: iansinnott/obsidian-claude-code-mcp` (or whichever the user chose)
- `Phase 7 (desktop mcp): completed at <ISO timestamp>`
- `second_brain_complete: true`
- `completed_at: <ISO timestamp>`

Append to `projects/second-brain/memory.md`:

```markdown
## <ISO timestamp> — Phase 7 complete: Claude Desktop MCP enabled
- Server: iansinnott/obsidian-claude-code-mcp
- Config file: <claude_desktop_config.json path>
- Desktop verification: Connectors panel shows "obsidian" connected
```

Tell the user:

> *"Phase 7 done. Both Claude Cowork AND Claude Desktop can now read your vault.*
>
> *Cowork uses direct filesystem access (same as Phases 0–5).*
> *Desktop uses the MCP server via WebSocket (port 22360).*
> *They don't conflict. Edits in either tool show up in both via the underlying vault files.*
>
> *cowork-obsidian install complete. Run `/second-brain build [life-area]` any time. Run `/onboard-second-brain` again to add another vault or revisit any phase."*

---

## Verification before calling done

- Claude Desktop's Connectors panel shows `obsidian` as connected
- A test query in Claude Desktop returns vault content
- State file shows `phase_7_desktop_mcp: enabled` and `second_brain_complete: true`

---

## Alternative MCP servers (advanced users only)

If `iansinnott/obsidian-claude-code-mcp` doesn't fit the user's setup, alternatives:

- [`MarkusPfundstein/mcp-obsidian`](https://github.com/MarkusPfundstein/mcp-obsidian) — requires the Obsidian Local REST API community plugin + an API key in env vars. More moving parts. Not recommended for newcomers.
- [`administrativetrick/obsidian-mcp`](https://github.com/administrativetrick/obsidian-mcp) — direct filesystem access, doesn't require Obsidian to be running. Different access model.
- [`marcelmarais/obsidian-mcp-server`](https://mcpservers.org/servers/marcelmarais/obsidian-mcp-server) — lightweight standalone, direct filesystem.

Each has different config requirements; consult that server's README. The Mac trap warning (`claude_desktop_config.json` is the only file Claude Desktop reads) applies to all of them.

---

## What this phase never does

- Edit `~/.claude/settings.json` or `~/.claude/mcp.json` for Claude Desktop (those are wrong — see Step 1)
- Recommend pulling the user's vault into the cloud for Desktop to read remotely (Desktop reads via local MCP server, not cloud)
- Force Phase 7 on users who only use Cowork (it's optional — most users skip it)
- Store any credentials or API keys in plain text in the config file (none of the recommended servers require credentials)
- Modify Cowork's filesystem wiring from Phase 4 (Phase 7 is purely additive)
