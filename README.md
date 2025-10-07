# Lattice CSS

A lightweight, flexible CSS utility library for modern web development.

Tiny, layout-first utilities for sites that prefer classless themes for typography/colors. Lattice focuses on **positioning** and **grid ergonomics**; use your theme for aesthetics.

## Features

- 🚀 **Lightweight** - Small footprint with only essential utilities
- 📦 **No Dependencies** - Pure CSS, no JavaScript required
- 🎨 **Flexible** - Comprehensive utilities for layout, spacing, typography, and more
- ⚡ **Fast** - Optimized minified version for production
- 📱 **Responsive Ready** - Flexbox and grid utilities included

## Installation

### Direct Download

Download `lattice.min.css` from this repository and include it in your HTML:

```html
<link rel="stylesheet" href="path/to/lattice.min.css">
```

### CDN (Coming Soon)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/lattice.min.css">
```

## Load order

```html
<!-- Always load CORE -->
<link rel="stylesheet" href="/css/lattice.css">

<!-- Optional: add EXTRA for more utilities -->
<link rel="stylesheet" href="/css/lattice.extra.css">

<!-- Optional: FULL for everything (advanced / power users) -->
<link rel="stylesheet" href="/css/lattice.full.css">
```

* Load **CORE** first.
* Add **EXTRA** when you need more helpers.
* Use **FULL** for “kitchen sink” layouts (advanced grid controls, spans, etc.).

---

## Guidance

* **Start with CORE** for blogs and simple sites. It’s intentionally small: page grid + internal grid + a few ergonomic helpers.
* **Add EXTRA** when you need spacing scales, responsive display, a11y helpers, flex/grid ergonomics, positioning, etc.
* **Use FULL** when you need **master column spans**, heavy grid control, or masonry-style layouts.

## Quick Start

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="lattice.min.css">
</head>
<body>
    <div class="flex items-center justify-center p-4">
        <h1 class="text-2xl font-bold text-gray-900">Hello, Lattice!</h1>
    </div>
</body>
</html>
```

## Documentation

Visit our [documentation site](https://rlnorthcutt.github.io/lattice/) for:

- Complete API reference
- Live examples
- Best practices

NEed to create docs
“Tip: Use col-span-* inside .grid, use col-start-* only with .grid-container.”

## Utility Classes

Lattice includes utilities for:

- **Layout**: Display, Flexbox, Grid, Position
- **Spacing**: Margin and Padding with consistent scale
- **Sizing**: Width and Height utilities
- **Typography**: Font sizes, weights, and alignment
- **Colors**: Text and background colors
- **Borders**: Width, style, radius, and colors
- **Effects**: Shadows and opacity
- **Interactivity**: Cursors, transitions, and user-select
- **Overflow**: Control overflow behavior
- **Z-Index**: Stacking order utilities
- **Gap**: Spacing for Grid and Flex layouts

## Naming & breakpoints

* **Hyphenated** naming: `sm-` (≥576px), `md-` (≥768px), `lg-` (≥1024px).
* **Master grid lines** use `c` (content) and `g` (gutter) internally. You don’t need to touch them unless you use `.col-start-*` (EXTRA/FULL).

---

## Copy/paste patterns

### 1) Page wrapper (CORE)

```html
<section class="grid-container">
  <header class="l-place-full">…full-bleed banner…</header>
  <main class="l-place-content">…centered article…</main>
</section>
```

### 2) Fluid cards (CORE)

```html
<div class="grid">
  <article>Card</article><article>Card</article><article>Card</article>
</div>
```

### 3) Fixed counts by breakpoint (CORE)

```html
<div class="grid sm-col-2 md-col-3 lg-col-4">
  <!-- 2 cols on small, 3 on medium, 4 on large -->
</div>
```

### 4) Center a narrow block (CORE)

```html
<div class="mx-auto max-w-100" style="max-width:60ch">…</div>
```

### 5) Responsive show/hide (EXTRA)

```html
<div class="d-block sm-d-none">Visible on xs only</div>
<div class="d-none md-d-block">Hidden until md+</div>
```

### 6) Accessibility (EXTRA)

```html
<a class="sr-only sr-only-focusable" href="#main">Skip to content</a>
```

### 7) Master grid fine control (FULL)

```html
<!-- Span across the first 8 master columns -->
<div class="col-span-8">Feature area</div>
```

---



## Development

The CSS is automatically minified via GitHub Actions when changes are pushed to the main branch.

## License

MIT License - feel free to use in your projects!

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.