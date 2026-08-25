# Lattice CSS

**Lattice** is a layout-first CSS utility toolkit. Core gives you a 12-column page shell and dead-simple component grids. Extra adds spacing, display, and advanced placement utilities—only when you need them.

> Bring your own styles. Lattice handles the layout math.

## Packages

| File                | Purpose                                                                                                                                                 | Load order                   |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| `lattice.css`       | **Core** page shell (`.lattice`), centered/bleed sections, a 12-track default `.grid`, and item width classes (`.col-*`).                               | Load first                   |
| `lattice.extra.css` | **Extra** spacing scale, responsive display toggles, grid flow/place helpers, **page-shell placement** (`.start-*`, `.span-*`), flex/positioning, a11y. | Load after Core              |
| `lattice.full.css`  | Core + Extra bundled together.                                                                                                                          | Use instead of the two above |

`dist/` contains minified builds for production/CDN.

```
dist/
├─ lattice.min.css
├─ lattice.extra.min.css
├─ lattice.full.css
└─ lattice.full.min.css
```

## Install

### CDN

```html
<!-- Core only -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/dist/lattice.min.css">

<!-- + Extra (optional) -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/dist/lattice.extra.min.css">
```

Or ship the one-file bundle:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/rlnorthcutt/lattice/dist/lattice.full.min.css">
```

### Self-host

```html
<!-- Always load Core first -->
<link rel="stylesheet" href="/css/lattice.css">
<!-- Optional helpers -->
<link rel="stylesheet" href="/css/lattice.extra.css">
```

## Quick start (copy/paste)

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<body class="lattice">
  <header class="full-width">
    <div class="p-4"><h1>Hello, Lattice</h1></div> <!-- .p-4 requires Extra -->
  </header>

  <main class="container">
    <!-- Equal columns: 1 col → 2 cols at md -->
    <div class="grid md-col-2 gap-3 my-4">
      <aside>Sidebar</aside>
      <article>Main content</article>
    </div>

    <!-- Unequal columns (requires Extra) -->
    <div class="grid gap-3">
      <aside class="md-span-4">Sidebar (4/12)</aside>
      <article class="md-span-8">Main content (8/12)</article>
    </div>
  </main>

  <footer class="full-width text-center p-3">Footer</footer>
</body>
```

* `.lattice` once at the page shell. Direct children default to contained width; add `.full-width` for full-bleed sections.
* Inside, use `.grid` for component grids. Set the **column count on the container**: `.grid.col-N` or `.grid.md-col-N`.
* For **unequal** columns, put `.span-*` on items (requires Extra).
* Breakpoints: `sm-`, `md-`, `lg-`, `xl-`, `xxl-`.

## Core concepts

### 1) Page shell (Core)

```html
<body class="lattice">
  <header class="full-width">...</header>
  <main class="container">...</main>
</body>
```

* Fluid outer gutters automatically include safe-area insets on notched devices.

### 2) Component grids (Core)

```html
<!-- Equal thirds: stack on mobile, 3 columns at md -->
<section class="grid md-col-3">
  <div>A</div>
  <div>B</div>
  <div>C</div>
</section>

<!-- Unequal layout (requires Extra): sidebar + content -->
<section class="grid">
  <aside class="md-span-4">Sidebar</aside>
  <main class="md-span-8">Content</main>
</section>
```

* `.grid` **defaults to 12 equal tracks**—no boilerplate needed.
* Set equal columns on the **container**: `.grid.col-N` or `.grid.md-col-N` (N = 1, 2, 3, 4, 6, 12).
* Set unequal spans on **items** with `.span-N` or `.md-span-N` (requires Extra).

### 3) Unequal column spans (Extra)

When you need non-uniform columns inside a `.grid`, use `.span-*` on child items:

```html
<div class="grid gap-3">
  <aside class="span-4">Sidebar (4/12)</aside>
  <main class="span-8">Content (8/12)</main>
</div>
```

* Use responsive variants: `sm-span-*`, `md-span-*`, etc. to change spans per breakpoint.
* Keep equal layouts using `.grid.col-N` on the container (Core).

## API reference (practical)

### Core classes

| Area              | Classes                                  | Notes                                                                    |
| ----------------- | ---------------------------------------- | ------------------------------------------------------------------------ |
| Page shell        | `.lattice`                               | One per page. Direct children default to contained width.                |
| Sections          | `.container`, `.full-width`              | Direct children of `.lattice`. `.container` is the default.              |
| Component grids   | `.grid`                                  | Defaults to 12 equal tracks.                                             |
| Equal columns     | `.grid.col-1,2,3,4,6,12`                | Set on the **container** to create N equal columns. Mobile-first.        |
| Responsive cols   | `.grid.sm-col-*`, `.grid.md-col-*`, …   | E.g. `<div class="grid md-col-3">` stacks on mobile, 3 cols at md+.     |
| Unequal spans     | `.span-1…12` (Extra)                    | Set on **items** inside `.grid` for 25/75, 3/9, etc. Requires Extra.    |

**Breakpoints (min-width):**
`sm 40rem` (640px), `md 48rem` (768px), `lg 64rem` (1024px), `xl 80rem` (1280px), `xxl 96rem` (1536px).
Classes use `xxl-` to avoid digit-leading selectors (docs can say “xxl”).

### Extra classes (high-value 95%)

| Area               | Examples                                                                                                  |
| ------------------ | --------------------------------------------------------------------------------------------------------- |
| Spacing            | `.p-0…5`, `.m-0…5`, `.gap-0…5`, `px-*`, `py-*`, etc.                                                      |
| Display            | `.d-none`, `.d-flex`, `.d-grid` + responsive `sm-*/md-*/lg-*/xl-*/xxl-*`                                |
| Grid flow/place    | `.flow-row`, `.flow-col`, `.flow-dense`, `.place-items-center`, `.place-content-start`, `.place-self-end` |
| Unequal spans      | `.span-1…12` + responsive `sm-span-*`, `md-span-*`, … (items inside `.grid`)                             |
| Flexbox            | `.flex-row`, `.justify-between`, `.items-center`, `.order-1`…                                             |
| Positioning & misc | `.relative`, `.sticky`, `.inset-0`, `.overflow-auto`, `.z-10`, `.aspect-video`                            |
| A11y               | `.sr-only`, `.visually-hidden`, `.sr-only-focusable`                                                      |
| Legacy helpers     | `.clearfix`, `.cf`                                                                                        |

## Customization

Override these tokens to match your rhythm:

```css
:root{
  /* Page container */
  --lat-container-width: 1024px;

  /* Master grid (12 content tracks via 1 + var(--lat-col-count)) */
  --lat-col-count: 11;
  --lat-fluid-area: max(0px, calc(50vw - var(--lat-container-width)/2));

  /* Component rhythm */
  --lat-space: 1rem;
  --lat-gap-x: 1rem;
  --lat-gap-y: 1rem;
}
```

> For safe-area support on iOS, include:
> `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`

## Recipes

**Full-bleed hero + contained content**

```html
<body class="lattice">
  <header class="full-width p-4">Hero</header>
  <main class="container">
    <div class="grid gap-3 my-4">
      <div class="col md-col-8">Main</div>
      <aside class="col md-col-4">Sidebar</aside>
    </div>
  </main>
  <footer class="full-width p-5">Footer</footer>
</body>
```

**Three cards (stack → thirds)**

```html
<div class="grid md-col-3 gap-3">
  <article>A</article>
  <article>B</article>
  <article>C</article>
</div>
```

**Sidebar + content (Extra)**

```html
<main class="container">
  <div class="grid gap-3">
    <aside class="md-span-3">Sidebar</aside>
    <section class="md-span-9">Content</section>
  </div>
</main>
```

## Development

* Source: `lattice.css`, `lattice.extra.css` at repo root.
* `dist/` builds are produced for releases and GitHub Pages.
* To test locally, reference source files in a simple HTML page.

Contributions welcome—issues and PRs encouraged.

## License

MIT © Ron Northcutt