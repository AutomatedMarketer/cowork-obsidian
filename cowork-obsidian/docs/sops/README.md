# Project 04 SOPs (v0.5.0)

Standard Operating Procedures for **VCInc cohort students** going through Project 04 — Second Brain in Obsidian (cowork-obsidian plugin v0.5.0).

Each lesson has both formats:
- **`SOP-CXX-NN.md`** — Markdown source (canonical, version-controlled, edit here)
- **`SOP-CXX-NN.docx`** — Word document for cohort delivery (auto-generated from `.md` via pandoc)

---

## The 4 docs (overview + 3 lessons)

| File | Title | Time | When to use |
|---|---|---|---|
| [SOP-C04-00](./SOP-C04-00.md) | Why a Second Brain | 10 min read | Conceptual primer. Read first. |
| [SOP-C04-01](./SOP-C04-01.md) | Setting Up Your Vault | 15–30 min | Install Obsidian, scaffold or connect a vault, wire to Cowork. |
| [SOP-C04-02](./SOP-C04-02.md) | How to Use It Day-to-Day | ~15 min | The daily loop: capture → wiki → output. With case studies by role. |
| [SOP-C04-03](./SOP-C04-03.md) | Maintenance and Growing | ~15 min | Weekly rhythm, sync, common pitfalls, growing the vault over time. |

Total time: ~60 min hands-on if scaffolding fresh, less if connecting to an existing vault.

---

## What changed from v0.3.0

v0.3.0 had 6 SOPs (00–05). v0.5.0 consolidates to 4 (00–03), aligning with the workspace source-of-truth. The previous Lessons 4 (Weekly Rhythm + `/open-vault`) and 5 (Use Cases & Daily Workflow) were merged into Lessons 02 (day-to-day, including case studies) and 03 (maintenance, weekly rhythm, sync, pitfalls).

| v0.3.0 | v0.5.0 | Change |
|---|---|---|
| SOP-C04-00 (Overview) | SOP-C04-00 (Why a Second Brain) | Rewritten as the conceptual "why" primer. |
| SOP-C04-01 (Connect or Create) | SOP-C04-01 (Setting Up Your Vault) | Streamlined install + scaffold/connect flow. |
| SOP-C04-02 (Three Operations) | SOP-C04-02 (How to Use It Day-to-Day) | **Consolidated.** Daily loop + case studies (was old Lesson 5). |
| SOP-C04-03 (Sync + Desktop MCP) | SOP-C04-03 (Maintenance and Growing) | **Consolidated.** Weekly rhythm + sync + pitfalls (was old Lessons 3 + 4). |
| SOP-C04-04 (Weekly Rhythm) | — | Merged into SOP-C04-03. |
| SOP-C04-05 (Use Cases & Daily Workflow) | — | Merged into SOP-C04-02. |

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

sops = ['SOP-C04-00', 'SOP-C04-01', 'SOP-C04-02', 'SOP-C04-03']

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
