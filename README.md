# Lattice CSS

Lattice is a layout-first CSS utility toolkit tuned for teams that already have typography and color opinions. The core layer gives you ergonomic grid primitives, predictable spacing, and sensible visibility helpers without forcing component styles on your project. Opt into the extra layers only when you need them.

> **Focus on structure.** Use your theme for aesthetics, and let Lattice handle the boring layout math.

## Packages at a Glance 

| File | Purpose | Notes |
| --- | --- | --- |
| `lattice.css` | **Core** 12-column page grid, fluid internal grids, and the smallest set of display/visibility helpers. | ~6 KB source / 2.5 KB minified. Ship this by default. |
| `lattice.extra.css` | Adds spacing scales, responsive display aliases, flex/grid helpers, accessibility utilities, and fine-grained spans. | Requires the core variables. ~17 KB source / 12 KB minified. |
| `lattice.full.css` | Pre-bundled core + extra for power users. Includes everything in one file. | ~23 KB source / 15 KB minified. |

The `dist/` directory contains the minified builds that are published via GitHub Pages and the CDN:

```text
dist/
├── lattice.min.css
├── lattice.extra.min.css
├── lattice.full.css
└── lattice.full.min.css
```

## Installation

### Use a CDN

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/dist/lattice.min.css">
```

Swap `lattice.min.css` for `lattice.extra.min.css` or `lattice.full.min.css` to include the higher tiers.

### Install Manually

Download the CSS bundle you need from the [`dist/`](dist) folder and copy it into your project. Reference it from your HTML just like any other stylesheet:

```html
<link rel="stylesheet" href="/assets/css/lattice.min.css">
```

When you self-host, keep the load order consistent:

```html
<!-- Always load CORE first -->
<link rel="stylesheet" href="/css/lattice.css">

<!-- Optional: add EXTRA for more helpers -->
<link rel="stylesheet" href="/css/lattice.extra.css">

<!-- Optional: FULL for everything in one file -->
<link rel="stylesheet" href="/css/lattice.full.css">
```

## Quick Start

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/dist/lattice.full.min.css">
  </head>
  <body class="grid-container">
    <header class="l-full-width">
      <div class="p-4">
        <h1>Hello, Lattice!</h1>
        <p class="m-0">A fluid hero using the master grid.</p>
      </div>
    </header>

    <main class="l-place-content grid gap-3 my-4">
      <article class="p-3">
        <h2>Fluid grid</h2>
        <p>Cards expand and wrap using <code>repeat(auto-fit, minmax(...))</code>.</p>
      </article>
      <article class="p-3">
        <h2>Responsive control</h2>
        <p>Add <code>sm-col-2</code>, <code>md-col-3</code>, <code>lg-col-4</code> to force column counts at breakpoints.</p>
      </article>
    </main>
  </body>
</html>
```

> The example above loads the **full** build so it can use spacing utilities such as `.p-4`, `.m-0`, `.gap-3`, and `.my-4`. If you only include the core layer, remove those classes or provide your own spacing styles.

## Core Concepts

- **Master grid:** `.grid-container` creates a 12-column layout with fluid gutters. Use `.l-full-width` for full-bleed sections and `.l-place-content` for centered content. [`lattice.css`](lattice.css)
- **Internal grids:** `.grid` sets up fluid cards by default. Add `.col-*` or breakpoint variants such as `.sm-col-2` to lock column counts. [`lattice.css`](lattice.css)
- **Responsive breakpoints:** Utilities with `sm-`, `md-`, and `lg-` prefixes target 576px, 768px, and 1024px min-width breakpoints. [`lattice.css`](lattice.css)
- **Spacing scale:** The extra layer exposes padding/margin utilities (`.p-0` → `.p-5`) and gap helpers derived from the `--m` rhythm variable. [`lattice.extra.css`](lattice.extra.css)
- **Flexbox & positioning:** `.d-flex`, alignment helpers, order utilities, and positioning shortcuts live in the extra layer. [`lattice.extra.css`](lattice.extra.css)
- **Accessibility:** `.sr-only` and `.sr-only-focusable` mirror common screen-reader patterns. [`lattice.extra.css`](lattice.extra.css)

You can adjust layout rhythm globally by overriding the CSS custom properties defined at the top of `lattice.css`:

```css
:root {
  --container-width: 1024px;   /* Page max width */
  --auto-col-min: 15rem;       /* Minimum card width in fluid grids */
  --column-count: 11;          /* Master grid content columns (+ 1 = 12 total) */
  --m: 1.5rem;                 /* Base spacing unit */
  --g: 1rem;                   /* Column gap */
  --gap-y: 1rem;               /* Row gap */
}
```

## Documentation & Examples

The `/docs` directory contains a static documentation site powered by Lattice itself. Open `docs/index.html` for the landing page, `docs/examples.html` for copy‑paste snippets, and `docs/docs.html` for API coverage. These are the same files published to [https://rlnorthcutt.github.io/lattice/](https://rlnorthcutt.github.io/lattice/).

## Development Workflow

- All source CSS lives at the project root (`lattice.css`, `lattice.extra.css`).
- Minified bundles in `dist/` are generated via GitHub Actions when commits land on the default branch.
- If you need to test changes locally, reference the source files directly in a demo HTML page or run the docs locally with any static file server.

Contributions are welcome—open an issue or submit a pull request with improvements.

## License

MIT © Ron Northcutt
