# Vault Folder Structure

Every vault built by `/onboard-second-brain` follows this exact shape:

```
<vault-root>/
│
├── about-me/                       (identity files, loaded by Cowork on every session)
│   ├── About Me.md
│   ├── Brand Voice.md
│   ├── AI Working Style.md
│   └── My Vault System.md          (written ~2 weeks after install)
│
├── <life-area-1>/                  (e.g. business/, work/, coaching/)
│   ├── raw/                        (USER writes here — brain dumps, half-formed ideas)
│   ├── wiki/                       (CLAUDE writes here — distilled, cross-linked pages)
│   └── output/                     (USER + CLAUDE collaborate here — finished deliverables)
│
├── <life-area-2>/
│   ├── raw/
│   ├── wiki/
│   └── output/
│
├── <life-area-3>/
│   ├── raw/
│   ├── wiki/
│   └── output/
│
└── <life-area-4>/                  (max 4 per vault)
    ├── raw/
    ├── wiki/
    └── output/
```

## The four invariants

1. **One vault root per Obsidian vault.** Don't nest vaults inside each other.
2. **Maximum 4 life areas per vault.** Macro beats micro. Subfolders inside `raw/` are fine for fine-grained topics.
3. **Exactly three subfolders per life area: `raw/`, `wiki/`, `output/`.** Lowercase. Same spelling everywhere. Predictable structure means generic prompts work for every life area forever.
4. **`about-me/` lives at the vault root.** Not inside a life area. Loaded into every Cowork session that operates on the vault.

## What goes where

| Folder | Who writes | What lives there | Example filenames |
|---|---|---|---|
| `raw/` | User only | Brain dumps, journal entries, article clippings, screenshot OCR, half-formed ideas | `2026-05-06-lesson-from-client-call.md`, `2026-05-06-pattern-on-pricing.md` |
| `wiki/` | Claude only | Short, focused, cross-linked pages — one topic per page | `Pricing strategy.md`, `Onboarding patterns.md`, `Sarah Mitchell case study.md` |
| `output/` | Both, collaboratively | Finished deliverables — strategy docs, client guides, distilled frameworks | `2026-05-06-Q3 strategy doc.md`, `Sarah's 90-day plan.md` |

## The `.obsidian/` folder

Inside your vault, Obsidian creates a hidden `.obsidian/` folder for app settings (theme, plugins, view preferences). **Leave it alone.** Don't tell Cowork to write there. If you back up the vault, include `.obsidian/` to preserve your settings.
