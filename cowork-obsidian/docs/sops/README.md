# Project 04 SOPs (v0.3.0)

Standard Operating Procedures for **VCInc cohort students** going through Project 04 — Second Brain in Obsidian (cowork-obsidian plugin v0.3.0).

Each lesson has both formats:
- **`SOP-CXX-NN.md`** — Markdown source (canonical, version-controlled, edit here)
- **`SOP-CXX-NN.docx`** — Word document for cohort delivery (auto-generated from `.md` via pandoc)

---

## The 6 docs (overview + 5 lessons)

| File | Title | Time | When to use |
|---|---|---|---|
| [SOP-C04-00](./SOP-C04-00.md) | Project 04: Your Second Brain in Obsidian (Overview) | 10 min read | Conceptual primer. Read first. |
| [SOP-C04-01](./SOP-C04-01.md) | Lesson 1: Connect or Create Your Vault | 5–40 min | Auto-detect existing vault OR scaffold a fresh one. Wire to Cowork. |
| [SOP-C04-02](./SOP-C04-02.md) | Lesson 2: The Three Operations of `/second-brain` | ~15 min | Run `/second-brain build`, `update`, `health-check` once. Skill-first. |
| [SOP-C04-03](./SOP-C04-03.md) | Lesson 3: Sync Across Machines (+ Optional Desktop MCP) | 15–35 min | Pick Obsidian Sync, Syncthing, or Git/GitHub. Optional Desktop MCP. |
| [SOP-C04-04](./SOP-C04-04.md) | Lesson 4: The Weekly Rhythm + `/open-vault` Daily Driver | ~20 min | Calendar reminders, home indexes, golden output, `My Vault System.md`. |
| [SOP-C04-05](./SOP-C04-05.md) | Lesson 5: Use Cases & Daily Workflow | ~15 min read | The "now what?" guide. 6 case studies by role + concrete workflow. |

Total time: ~90 min hands-on if scaffolding fresh, less if connecting to an existing vault.

---

## What changed from v0.1.0 / v0.2.0

The original Project 04 SOPs (v0.1.0) were 4 files. v0.3.0 reorganizes to 6 files: overview + 5 lessons. Two new lessons (Sync, Use Cases) and a major rewrite of Lessons 2 + 4 to be skill-first instead of paste-the-prompt.

| Original (v0.1.0) | v0.3.0 | Change |
|---|---|---|
| SOP-C04-00 (Overview) | SOP-C04-00 (Overview) | Refreshed: new plugin name, three vault modes, soft prereq, 6-doc map, the 3 skills the plugin installs |
| SOP-C04-01 (Install + Build Vault) | SOP-C04-01 (Connect or Create) | Now covers BOTH the "connect existing vault" branch AND the "scaffold fresh vault" branch |
| SOP-C04-02 (Three Prompts — manual paste) | SOP-C04-02 (Three Operations of `/second-brain` — skill-first) | **Major rewrite.** v0.1.0 had students paste a 200-word prompt manually. v0.3.0 uses the `/second-brain` skill the plugin installs. ~15 min vs ~30 min. Manual prompt templates moved to an appendix for reference only. |
| — | SOP-C04-03 (Sync + Desktop MCP) **NEW in v0.2.0** | Brand new lesson covering all three sync options + the optional GoHighLevel-trap-aware Desktop MCP setup |
| SOP-C04-03 (Weekly Rhythm + Custom Skill) | SOP-C04-04 (Weekly Rhythm + `/open-vault` Daily Driver) | **Major rewrite.** v0.1.0 had students BUILD a custom "Vault Wiki Workflow" skill via Skill Creator. v0.3.0 drops that — the plugin already has `/second-brain` and now adds `/open-vault` as the daily-driver. ~20 min vs ~30 min. |
| — | SOP-C04-05 (Use Cases & Daily Workflow) **NEW in v0.3.0** | Read-only "now what?" guide. 6 case studies by role (coach, agency owner, course creator, consultant, writer, multi-business operator) + references to second-brain thinkers (Julie Chenell, Tiago Forte, Andy Matuschak) + concrete daily/weekly/monthly workflow examples. |

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

sops = ['SOP-C04-00', 'SOP-C04-01', 'SOP-C04-02', 'SOP-C04-03', 'SOP-C04-04', 'SOP-C04-05']

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
