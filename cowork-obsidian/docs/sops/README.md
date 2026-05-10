# Project 04 SOPs (v0.2.0)

Standard Operating Procedures for **VCInc cohort students** going through Project 04 — Second Brain in Obsidian (cowork-obsidian plugin v0.2.0).

Each lesson has both formats:
- **`SOP-CXX-NN.md`** — Markdown source (canonical, version-controlled, edit here)
- **`SOP-CXX-NN.docx`** — Word document for cohort delivery (auto-generated from `.md` via pandoc)

---

## The 5 SOPs

| File | Title | Time | When to use |
|---|---|---|---|
| [SOP-C04-00](./SOP-C04-00.md) | Project 04: Your Second Brain in Obsidian (Overview) | 10 min read | Conceptual primer. Read first. |
| [SOP-C04-01](./SOP-C04-01.md) | Lesson 1: Connect or Create Your Vault | 5–40 min | Auto-detect existing vault OR scaffold a fresh one. Wire to Cowork. |
| [SOP-C04-02](./SOP-C04-02.md) | Lesson 2: The Three Prompts That Run Your Second Brain | 30 min | Run `build`, `update`, `health-check` for the first time. Mode-aware. |
| [SOP-C04-03](./SOP-C04-03.md) | Lesson 3: Sync Across Machines (+ Optional Desktop MCP) | 15–35 min | Pick Obsidian Sync, Syncthing, or Git/GitHub. Optional Desktop MCP. |
| [SOP-C04-04](./SOP-C04-04.md) | Lesson 4: The Weekly Rhythm and the Custom Skill | 30 min | Install slash-command skill, set calendar reminders, write `My Vault System.md`. |

Total time: 100–125 minutes if scaffolding fresh, less if connecting to an existing vault.

---

## What changed from v0.1.0

The original Project 04 SOPs were 4 files (overview + 3 lessons). v0.2.0 reorganizes to 5 files (overview + 4 lessons), with a new **Lesson 3 — Sync** that didn't exist before.

| v0.1.0 | v0.2.0 | Change |
|---|---|---|
| SOP-C04-00 (Overview) | SOP-C04-00 (Overview, refreshed) | Refreshed for the renamed plugin (`cowork-obsidian`), three vault modes, soft prereq, 5-lesson map |
| SOP-C04-01 (Install + Build Vault) | SOP-C04-01 (Connect or Create) | Now covers BOTH the "connect existing vault" branch AND the "scaffold fresh vault" branch |
| SOP-C04-02 (Three Prompts) | SOP-C04-02 (Three Prompts, mode-aware) | Updated for mode awareness (`scaffold`, `overlay`, `external`) and the new use-case picker |
| — | SOP-C04-03 (Sync + Desktop MCP) **NEW** | Brand new lesson covering all three sync options + the optional GoHighLevel-trap-aware Desktop MCP setup |
| SOP-C04-03 (Weekly Rhythm) | SOP-C04-04 (Weekly Rhythm, refreshed) | Same content, refreshed for v0.2.0 plugin name + mode-aware skill |

---

## Re-generating the `.docx` files

The `.docx` files are generated from the `.md` sources via [pandoc](https://pandoc.org). To regenerate after editing the markdown:

### Using `pypandoc` (no system install needed)

```bash
python -m pip install pypandoc_binary
```

Then run:

```python
import pypandoc

sops = ['SOP-C04-00', 'SOP-C04-01', 'SOP-C04-02', 'SOP-C04-03', 'SOP-C04-04']

for sop in sops:
    pypandoc.convert_file(
        f'{sop}.md',
        'docx',
        outputfile=f'{sop}.docx',
        extra_args=['--standalone', '--toc', '--toc-depth=2'],
    )
```

### Using a system `pandoc` install

```bash
for f in SOP-C04-*.md; do
  pandoc "$f" -o "${f%.md}.docx" --standalone --toc --toc-depth=2
done
```

(Mac: `brew install pandoc`. Windows: `winget install JohnMacFarlane.Pandoc`. Linux: `sudo apt install pandoc`.)

---

## Voice + format conventions

These SOPs follow the original v0.1.0 voice (Nuno Tavares for VCInc):

- **Direct, opinionated.** No corporate fluff. Short emphatic sentences.
- **Numbered top-level sections** with emoji icons (🎯 🧠 📦 🤖 ✅ 💪 🛡 🔍 🎓 ⚠ ➡).
- **Paste-ready prompts** in code blocks for the Cowork-driven steps.
- **"If something doesn't work" tables** with verbatim user-quotes for what to tell Claude.
- **"Under the hood — for the curious"** sections for students who want to know *why*.
- **"Teaching notes (for the instructor)"** at the end for cohort facilitators.

When editing, keep the section structure consistent across SOPs — students rely on knowing where to find each piece.

---

## License + Built by

[MIT](../../../LICENSE) — yours forever.

[Nuno Tavares](https://nunomtavares.com) for [VCInc](https://vcinc.com) · Newsletter: [automatedmarketer.net](https://automatedmarketer.net) · YouTube: [@AutomatedMarketer](https://youtube.com/@AutomatedMarketer)
