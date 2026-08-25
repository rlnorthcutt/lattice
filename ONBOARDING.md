# lattice — Onboarding Guide

## Purpose

Lattice is a lightweight, layout-first CSS utility toolkit. Its primary deliverable is `lattice.css`, a standalone 12-column grid system with fluid gutters and responsive breakpoints. A companion file, `lattice.extra.css`, adds spacing, flexbox, display, overflow, aspect-ratio, and accessibility helpers. Together they form a zero-dependency stylesheet pair (or a combined "full" bundle) that developers drop in via CDN or npm to handle common layout work without pulling in a heavyweight framework.

---

## Architecture

The project is intentionally small — two source CSS files and a static documentation site.

**Source files**
- `lattice.css` — the core library. Defines the 12-column grid via CSS custom properties (`--lat-*` tokens), component-level grid helpers, and responsive equal-column shortcuts across five breakpoints (`sm` / `md` / `lg` / `xl` / `xxl`).
- `lattice.extra.css` — the utility extension. Builds a spacing scale (0–5) on top of a single `--lat-space` base token and adds display, flex, grid-span, positioning, overflow, and aspect-ratio classes, each with responsive variants.

**Distribution bundles** (generated, not hand-edited)
- `core` — `lattice.css` alone.
- `extra` — `lattice.extra.css` alone.
- `full` — both files concatenated and minified.

**Documentation site** (`docs/`)
A three-page static HTML site (`index.html`, `core.html`, `extra.html`) hosted on GitHub Pages. It bundles its own minified stylesheet (`docs.min.css`) and a small dark-mode toggle script. Syntax highlighting is handled by Prism.js.

**CI/CD** (`.github/workflows/`)
- `minify-css.yml` — triggers on changes to the source CSS files on `main`, runs `clean-css-cli` to produce minified bundles in `dist/`, copies assets into `docs/`, and commits the results back to the repo.
- `doc-assets.yml` — handles any additional asset propagation for the documentation site.

---

## Key Patterns

**CSS custom properties as the single source of truth.** All sizing, spacing, and column counts flow through `--lat-*` tokens. Customization means overriding tokens in a `:root` block — there is no build step required for consumers.

**No JavaScript at runtime.** The library itself is pure CSS. The only JS in the repo (`dark-mode-toggle.js`, `prism.js`) belongs exclusively to the documentation site.

**Generated artifacts committed to the repo.** Minified files under `docs/static/` and any future `dist/` output are produced by CI and committed directly. Do not edit these by hand — edit the source files and let the workflow regenerate them.

**Flat, explicit class naming.** Classes follow a readable `lat-*` prefix convention (e.g., `lat-grid`, `lat-col-6`, `lat-mt-3`) with breakpoint suffixes appended for responsive variants. There is no CSS preprocessor or build tool involved in authoring.

**Separation of "core" and "extra."** Core is grid-only and stable; extra is additive utilities. This split lets projects include only what they need and makes it easy to reason about which file a class comes from.

---

## Getting Started

1. **Read `README.md`** first — it is the authoritative API reference, covering installation options, all utility families, token customization, and common recipes.
2. **Open `lattice.css`** to understand the grid foundation: how the 12-column system is defined, how breakpoints work, and which tokens are available for overriding.
3. **Open `lattice.extra.css`** to see how the spacing scale and utility helpers are built on top of those same tokens.
4. **Browse `docs/index.html`** (or the live GitHub Pages site) for live markup examples that demonstrate practical usage patterns.
5. **Check `TODO.md`** for known bugs and gaps before starting any new work — it gives a quick picture of where the project currently stands.
6. **Review `.github/workflows/minify-css.yml`** if you need to change the build or release process.

## Repository Statistics

- **Total files:** 19
- **Total lines:** 2793

| Language | Files | Lines | % |
|----------|------:|------:|--:|
| CSS | 7 | 1485 | 53.2% |
| HTML | 3 | 532 | 19.0% |
| JavaScript | 3 | 356 | 12.7% |
| Markdown | 2 | 221 | 7.9% |
| YAML | 2 | 168 | 6.0% |

## Documentation Library

- `README.md` — Project overview and getting started guide
- `TODO.md` — Documentation

## File Tree

```
lattice/
├── .github/
│   └── workflows/
│       ├── doc-assets.yml
│       └── minify-css.yml
├── .gitignore
├── README.md
├── TODO.md
├── docs/
│   ├── core.html
│   ├── css/
│   │   ├── ivy.full.css
│   │   ├── lattice.full.css
│   │   ├── prism.css
│   │   └── zzOverrides.css
│   ├── extra.html
│   ├── index.html
│   ├── js/
│   │   ├── dark-mode-toggle.js
│   │   └── prism.js
│   └── static/
│       ├── docs.min.css
│       └── docs.min.js
├── lattice.code-workspace
├── lattice.css
└── lattice.extra.css
```
