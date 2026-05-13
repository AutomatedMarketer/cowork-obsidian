---
title: Install Walkthrough — What the Wizard Asks and Why
lesson: 01b
project: 04-second-brain-obsidian
reading_level: 3rd-4th grade
last_updated: 2026-05-13
---

# Install Walkthrough — What the Wizard Asks and Why

When you run `/onboard-second-brain`, the wizard walks you through 9 short phases. Each phase asks a question or two. This guide tells you what the wizard will ask, why it asks, what a good answer looks like, and what you will see next. Read this once before you run the wizard, then run it with this guide open in another tab.

## How long this takes

About 10 to 15 minutes on a fresh machine. The wizard saves your spot. So if your kid walks in or you spill coffee, you can run the same command later and pick up where you stopped.

## Before you start

- cowork-ai-os installed and onboarded (so `about-me/` files exist for the wizard to read)
- Obsidian downloaded (Phase 2 will walk you through this if you skipped it)
- Pen and paper or a notes app open — the wizard might surface things worth jotting down

---

## Phase 0 — Welcome

**What this phase does:** The wizard greets you. It tells you what you are about to build. It scaffolds a small project folder for the second brain. It saves a state file so you can pause and resume.

**What the wizard asks:**
- "Want to see a sample `vaults.md` first, or jump in?"
- Soft check: Did you finish Project 01 (Core Setup)? Yes or no — both are fine.

**Why we ask:** A welcome sets expectations. We name the time cost up front (about 10 to 15 minutes). We give you a chance to bail if today is not the day. The sample option is for visual learners — seeing one filled-in config makes the planning questions land faster. The Project 01 check is soft because we can write fresh identity files later if you skipped it.

**Example answer:** Type `let's go` to start. Or `show me a sample` if you want to see what a finished config looks like first.

**What you'll see next:** The wizard creates `projects/second-brain/` and a memory log file. Then it tells you Phase 0.5 is the fork in the road.

---

## Phase 0.5 — Connect or Create

**What this phase does:** The wizard auto-finds any Obsidian vaults already on your machine. It reads Obsidian's own config file. Then it asks if you want to connect to one or start fresh.

**What the wizard asks:**
- "I found these Obsidian vaults — pick one (1 to N), paste a path manually (z), or skip and create new (n)."
- If you connect: "What is the purpose of this vault?" (one short sentence)
- If you connect: "Want to add a `raw/wiki/output` overlay on one sub-folder? Yes or no."

**Why we ask:** If you already have a vault, we do not want to make a duplicate or overwrite your notes. So we ask first. The three modes mean very different things. **External** means we read your vault but do not change its layout. **Overlay** means we add the second-brain folders inside one sub-folder of your vault. **Scaffold** means we build a fresh new vault from scratch.

**Example answer:** If you have a vault called `Hermes Second Brain`, pick its number from the list. If you do not have one yet, type `n` to scaffold a fresh one.

**What you'll see next:** If you connected, the wizard skips Phases 1, 2, and 3. It jumps straight to Phase 4. If you chose new, it moves to Phase 1.

---

## Phase 1 — Plan Vaults

**What this phase does:** The wizard asks 4 short questions. Then it writes a `vaults.md` config file. This file lists every vault you will use and what life areas live inside.

**What the wizard asks:**
- "One vault or multiple?" (most people pick one)
- "What 1 to 4 life areas should live in this vault?"
- "Copy your `about-me/` files from your Cowork workspace, or write fresh ones?"
- A heads-up that Phase 3 will seed 2 to 3 raw notes for the first build

**Why we ask:** We plan first so the wizard does not dump everything in one giant folder. We help you pick how many life areas you will have before we make any folders. **Max 4 life areas per vault** — Claude mixes things up beyond that. Macro beats micro. Subfolders inside `raw/` are fine for finer topics later.

**Example answer:** "One vault. Life areas: `coaching`, `agency`, `writing`, `personal`. Copy about-me from the workspace."

**What you'll see next:** The wizard shows you a draft `vaults.md` in a code block. You verify it. It saves. Then it previews Phase 2.

---

## Phase 2 — Install Obsidian

**What this phase does:** The wizard checks if Obsidian is already on your machine. If not, it walks you through the download and install. Then it makes sure Obsidian launches.

**What the wizard asks:**
- "Do you already have Obsidian installed? Yes, no, or not sure?"
- "Tell me when the installer finishes."
- "Did the welcome dialog appear?"

**Why we ask:** Some students skipped the install step in the install guide. The wizard checks and walks them through if needed. We also stop you from picking a vault location inside Obsidian — we want to do that in Phase 3, where we can refuse cloud-sync paths.

**Example answer:** "Yes, already installed." Or "Just installed, the welcome dialog is up."

**What you'll see next:** The wizard confirms Obsidian launches cleanly. Then it previews Phase 3 — the scaffold.

---

## Phase 3 — Scaffold First Vault

**What this phase does:** The wizard asks where the vault should live. It checks the path against a cloud-sync block list. Then it builds the folder structure. It copies your `about-me/` files in. It seeds 2 to 3 raw notes. It drops a `wiki/hot.md` cache file.

**What the wizard asks:**
- "What OS are you on?"
- "Is this safe path OK, or do you want a different one?"
- "Pick one life area to seed — what is a half-formed idea or recent lesson?" (give 2 or 3)
- "Now switch to Obsidian. File → Open another vault. Confirm what you see."

**Why we ask:** We ask for the path FIRST because we have to refuse cloud-sync paths before creating any folders. Cloud sync corrupts vaults 95% of the time — duplicate files, missing notes, phantom edits. We drop a `wiki/hot.md` template so Claude has a small "always on" memory at session start. Without that file, Claude has to scan your whole vault every prompt. With it, the first session loads in seconds.

**Example answer:** "Mac. Use `~/SecondBrain/`." Then for raw notes: "Lesson from today's client call: the script worked but the offer was wrong."

**What you'll see next:** The wizard shows you the folder tree on disk. You open Obsidian and confirm the file tree on the left matches. Then it previews Phase 4.

---

## Phase 4 — Wire Cowork to Vault

**What this phase does:** The wizard adds your vault path to a `safe-zones.md` file. This is the only file that tells Cowork which folders it can touch. Then the wizard runs a read test and a write test. It also offers to install the official `obsidian:` skill pack.

**What the wizard asks:**
- "OK to add the vault to safe-zones?"
- "OK to do a read test on this raw note?"
- "OK to write a tiny test note to your `output/` folder, then delete it?"
- "Install the official `obsidian:` skill pack? (Recommended.)"

**Why we ask:** Cowork can read every file on your computer in theory. We do not want that. The safe-zones file says: "Only the `/second-brain` skill can touch this vault path. Other skills must not." Without this, `/tidy-downloads` could move files inside your vault by accident. The skill pack matters because Obsidian's Bases format keeps changing. The pack teaches Claude the current syntax so your `.base` files do not break.

**Example answer:** Say yes to both tests. Say `install obsidian-skills` for the skill pack.

**What you'll see next:** The wizard reads one of your seeded raw notes back to you. Then it writes a test file you can see appear in Obsidian. Then it deletes it. Then it previews Phase 5.

---

## Phase 5 — The Three Prompts

**What this phase does:** The wizard runs `/second-brain build` for the first time. It reads your seeded raw notes. It builds your first wiki pages. Then it explains `update` and `health-check` for future use. It also asks when you want to run weekly updates.

**What the wizard asks:**
- "Ready to run `build` against your seeded life area?"
- "Approve each wiki draft before I save?"
- "When do you want to run `update` weekly? Sunday evening, Friday afternoon, Monday morning, or ad-hoc?"
- "Want me to draft `My Vault System.md` now or in two weeks?"

**Why we ask:** We run `build` once during install to give you the "oh, it works" moment immediately. Many students need to see the vault produce real wiki pages before they trust the system. We save the three prompt templates so you can run `update` and `health-check` later without remembering the wording. The weekly rhythm question matters — a system without a rhythm dies in 3 weeks.

**Example answer:** "Yes, build it. Sunday evening for the weekly. Skip `My Vault System.md` for now."

**What you'll see next:** The wizard shows you each wiki page draft. You approve. The pages save into `wiki/`. Then it tells you the install is complete (or moves to Phase 6 if you want sync).

---

## Phase 6 — Sync Setup (Optional)

**What this phase does:** The wizard helps you sync the vault across more than one computer. Three options. A 2-question decision tree picks the right one.

**What the wizard asks:**
- "Will you also use this on iPad, iPhone, or Android?"
- "Free or paid?"
- Then walks you through the chosen option (Obsidian Sync, Syncthing, or Git)

**Why we ask:** Sync is OPTIONAL. Most students do not need it. We only set it up if you actually use a second machine. We also explain the encryption key warning very clearly. Obsidian Sync's key is unrecoverable — lose it and your notes are gone forever. So we make you save it in a password manager BEFORE pasting it into Obsidian.

**Example answer:** "Skip Phase 6 for now." Or if you want it: "Mobile yes, paid is fine — pick Obsidian Sync."

**What you'll see next:** If you skipped, the wizard moves to Phase 7. If you set up sync, it walks you through the chosen option step by step. Then it previews Phase 7.

---

## Phase 7 — Claude Desktop MCP (Optional)

**What this phase does:** The wizard wires Claude Desktop (the chat app) to read your vault too. Two paths. Path A is the default. Path B is for power users.

**What the wizard asks:**
- "Pick A unless you have a reason not to. A or B?"
- "OK to back up your existing config file before I edit it?"
- "Restart Claude Desktop now. Type 'list 3 files in my vault' and tell me what Claude says."

**Why we ask:** We offer two paths. **Path A** uses the Anthropic Filesystem MCP. It works in 1 minute on every operating system. It reads every vault file. **Path B** uses the iansinnott bridge. It takes 3 more minutes. It also lets Claude see whichever note you currently have open in Obsidian. The default is Pick A because in a 2-day workshop, Path A succeeds for 95% of students. You can always upgrade to Path B later.

**Example answer:** "Path A." Then after the restart: "Claude listed three files from my vault."

**What you'll see next:** The wizard validates the config file. It tells you the install is complete. Both Cowork and Claude Desktop can now read your vault.

---

## After the wizard

When the wizard finishes, run `/open-vault`. It will read your hot cache and tell you the vault is alive. Drop your first raw note — anything from your day. Then go to lesson 02 to learn the daily rhythm.

## Common questions students ask DURING the install

**"Can I change my answers later?"**
Yes. Run `/onboard-second-brain` again. The wizard knows what you have already done. It lets you redo specific phases.

**"What if Claude gets stuck?"**
Tell Claude what you see. It will help you finish the phase or skip it for now.

**"I picked the wrong mode by accident."**
Run `/onboard-second-brain` again. Pick the right mode this time. Your old folders are safe.

**"Can I run this on more than one computer?"**
Yes — but each computer needs the wizard run once. Use the same vault path on each. Or use Obsidian Sync to keep them in step.

**"How do I know it worked?"**
Run `/open-vault`. If Claude lists your vault and says how many notes you have, it worked.

---

- Previous: [SOP-C04-01 Setting up your vault](SOP-C04-01.md)
- Next: [SOP-C04-02 How to use it day-to-day](SOP-C04-02.md)
- Quick reference: [Repo README](../../README.md)
- Watch: Nuno's install video (placeholder URL: TBD-by-Nuno)
