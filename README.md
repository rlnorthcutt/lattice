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
    <div class="grid gap-3 my-4">
      <aside class="col md-col-4">Sidebar</aside>
      <article class="col md-col-8">Main content</article>
    </div>
  </main>

  <footer class="full-width text-center p-3">Footer</footer>
</body>
```

* `.lattice` once at the page shell.
* Its **direct children** pick **bleed** (`.full-width`) or **contained** (`.container`).
* Inside, use `.grid` for components; items size with `.col-*` (mobile-first).
* `.col` = `.col-12`. Breakpoints: `sm-`, `md-`, `lg-`, `xl-`, `2xl-` (2xl).

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
<section class="grid">
  <div class="col md-col-4">A</div>
  <div class="col md-col-4">B</div>
  <div class="col md-col-4">C</div>
</section>
```

* `.grid` **defaults to 12 tracks**—no boilerplate needed.
* Items: `.col-1,2,3,4,6,8,10,12` (+ responsive `sm-`, `md-`, `lg-`, `xl-`, `2xl-`).

### 3) Page-level placement (Extra)

For precise art-direction on the **master grid**:

```html
<main class="lattice">
  <aside  class="start-1 span-3">Nav</aside>
  <section class="start-4 span-9">Main</section>
</main>
```

* **Only** for **direct children of `.lattice`** (enforced in CSS).
* Inside `.grid`, keep using `.col-*`.

## API reference (practical)

### Core classes

| Area            | Classes                                        | Notes                                               |
| --------------- | ---------------------------------------------- | --------------------------------------------------- |
| Page shell      | `.lattice`                                     | One per page. Provides fluid gutters + named lines. |
| Sections        | `.container`, `.full-width`                    | Must be direct children of `.lattice`.              |
| Component grids | `.grid`                                        | Defaults to 12 equal tracks.                        |
| Item widths     | `.col` (= `.col-12`), `.col-1,2,3,4,6,8,10,12` | Mobile-first spans inside `.grid`.                  |
| Responsive      | `sm-`, `md-`, `lg-`, `xl-`, `2xl-`           | E.g. `md-col-4`.                                    |

**Breakpoints (min-width):**
`sm 40rem` (640px), `md 48rem` (768px), `lg 64rem` (1024px), `xl 80rem` (1280px), `2xl 96rem` (1536px).
Classes use `2xl-` to avoid digit-leading selectors (docs can say “2xl”).

### Extra classes (high-value 95%)

| Area                     | Examples                                                                                                  |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| Spacing                  | `.p-0…5`, `.m-0…5`, `.gap-0…5`, `px-*`, `py-*`, etc.                                                      |
| Display                  | `.d-none`, `.d-flex`, `.d-grid` + responsive `sm-*/md-*/lg-*/xl-*/2xl-*`                                |
| Grid flow/place          | `.flow-row`, `.flow-col`, `.flow-dense`, `.place-items-center`, `.place-content-start`, `.place-self-end` |
| **Page-shell placement** | `.start-1…12`, `.span-1…12` (only on `.lattice > *`)                                                      |
| Flexbox                  | `.flex-row`, `.justify-between`, `.items-center`, `.order-1`…                                             |
| Positioning & misc       | `.relative`, `.sticky`, `.inset-0`, `.overflow-auto`, `.z-10`, `.aspect-video`                            |
| A11y                     | `.sr-only`, `.sr-only-focusable`                                                                          |
| Legacy helpers           | `.clearfix`, `.cf`                                                                                        |

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
<div class="grid gap-3">
  <article class="col md-col-4">A</article>
  <article class="col md-col-4">B</article>
  <article class="col md-col-4">C</article>
</div>
```

**Designer slice on the page shell (Extra)**

```html
<main class="lattice">
  <section class="start-3 span-6">Centered feature</section>
</main>
```

## Development

* Source: `lattice.css`, `lattice.extra.css` at repo root.
* `dist/` builds are produced for releases and GitHub Pages.
* To test locally, reference source files in a simple HTML page.

Contributions welcome—issues and PRs encouraged.

## License

MIT © Ron Northcutt