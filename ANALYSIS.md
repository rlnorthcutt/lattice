# File Summaries

## [core] Core

### `lattice.css`

Lattice.css is a lightweight CSS layout utility library providing a 12-column master grid system with fluid gutters, component-level grid helpers, and minimal positioning/display utilities. It uses CSS custom properties for configuration and supports responsive breakpoints (sm/md/lg/xl/xxl) for equal-column layouts. The file is self-contained and standalone, requiring no dependencies.

**Suggestions:**

- [+] **improvement** | file: `lattice.css` | effort: trivial
  The comment says '--lat-col-count:11; /* 1 + 11 = 12 content cols */' but this is confusing — rename the variable to '--lat-col-gap-count' or add a clearer inline explanation distinguishing between column tracks and gap tracks to avoid misreading the grid-template-columns formula.
  `done_when:` The variable name or comment unambiguously explains why the value is 11 for a 12-column grid

- [+] **improvement** | file: `lattice.css` | effort: small
  The responsive equal-column breakpoints are inconsistent: sm/md lack col-6/col-12, lg lacks col-2/col-6, xl lacks col-2/col-3/col-6, and xxl only has col-6. Audit and fill gaps so all meaningful column counts are available at every breakpoint, or document the intentional omissions.
  `done_when:` Each breakpoint either has a full set of col-* variants or a code comment explicitly states which counts are excluded and why

- [+] **improvement** | file: `lattice.css` | effort: small
  The .grid utility has no mobile-first single-column fallback — on narrow viewports a 12-column default grid can cause overflow. Add a base .grid.col-1 default or a <sm media query that resets --lat-cols to repeat(1, minmax(0,1fr)).
  `done_when:` Rendering .grid with no col-* class on a 320px viewport shows a single-column layout with no horizontal overflow

- [d] **docs** | file: `lattice.css` | effort: trivial
  The breakpoint values (40rem, 48rem, 64rem, 80rem, 96rem) are only labeled in comments. Expose them as CSS custom properties (e.g. --lat-bp-sm: 40rem) in :root so consumers can reference them in their own media queries for consistency.
  `done_when:` All five breakpoint values are declared as --lat-bp-* variables in :root and the media queries reference those variables (or the comments cross-reference them)

- [>] **performance** | file: `lattice.css` | effort: trivial
  All utility classes use !important unconditionally. Evaluate whether !important is actually needed for each rule — overuse increases specificity debt and makes overriding legitimate in downstream CSS harder. At minimum, document why each !important is required.
  `done_when:` Each !important declaration either has a comment justifying it or has been removed where unnecessary

---

## [build] Build

### `.github/workflows/minify-css.yml`

GitHub Actions workflow that triggers on pushes to main when CSS source files change. It installs clean-css-cli, minifies individual and combined CSS bundles into a dist/ directory, copies assets to the docs/ folder for GitHub Pages, and commits the generated artifacts back to the repository.

**Suggestions:**

- [+] **improvement** | file: `.github/workflows/minify-css.yml` | effort: trivial
  The 'Copy minified full to docs' step copies lattice.full.css (non-minified) instead of lattice.full.min.css to docs/css/. This likely means GitHub Pages serves unminified CSS, defeating the purpose of the build step.
  `done_when:` The cp command reads: cp dist/lattice.full.min.css docs/css/

- [!] **security** | file: `.github/workflows/minify-css.yml` | effort: small
  Committing generated build artifacts back to the source branch via git push creates a push loop risk and pollutes history. Consider using a dedicated orphan branch or GitHub Pages artifact upload (actions/upload-pages-artifact) to separate source from built output.
  `done_when:` The workflow uses actions/upload-pages-artifact and actions/deploy-pages instead of git commit + git push, or pushes to a dedicated branch like gh-pages.

- [+] **improvement** | file: `.github/workflows/minify-css.yml` | effort: trivial
  The workflow is missing a GitHub Pages deployment step (actions/deploy-pages) despite having the pages environment, id-token write permission, and a url output reference — the deployment never actually happens.
  `done_when:` A step using actions/deploy-pages@v4 exists and references the upload artifact step, and the page_url output is populated on successful runs.

- [>] **performance** | file: `.github/workflows/minify-css.yml` | effort: trivial
  clean-css-cli is installed globally via npm each run with no caching. Pin the version in a package.json and use actions/setup-node cache: 'npm' to avoid redundant network installs.
  `done_when:` actions/setup-node includes 'cache: npm' and clean-css-cli is listed in package.json devDependencies.

---

## [util] Util

### `lattice.extra.css`

A utility CSS library that extends the core Lattice.css framework with spacing, display, grid, flex, positioning, overflow, aspect-ratio, and accessibility helper classes. It uses a 0–5 scale built on CSS custom property fraction tokens derived from a base --lat-space variable. Responsive variants are provided for display and grid-span utilities across five breakpoints.

**Suggestions:**

- [d] **docs** | file: `lattice.extra.css` | effort: trivial
  The file is truncated mid-rule (.flex-col{flex-) — ensure the full file is committed and distributed, as the truncation will cause a CSS parse error in production.
  `done_when:` Running a CSS linter (e.g. `stylelint lattice.extra.css`) reports zero parse errors and the file ends with a complete closing brace.

- [+] **improvement** | file: `lattice.extra.css` | effort: small
  Add .mx-auto and .my-auto utility classes for centering elements, a common need that is missing from the margin scale.
  `done_when:` grep -q 'mx-auto' lattice.extra.css returns 0.

- [>] **performance** | file: `lattice.extra.css` | effort: medium
  The spacing scale repeats the same var() references across ~120 rules. Consider using a CSS @layer and generating these via a build step (e.g. PostCSS) to reduce file size and make the scale easier to extend without manual duplication.
  `done_when:` A build script exists that generates the spacing/gap/margin/padding rules from a single token definition, and the output file size is measurably smaller than the hand-authored version.

- [+] **improvement** | file: `lattice.extra.css` | effort: small
  Wrap utility classes in a @layer utilities block so authors can override them from unlayered styles without needing !important, reducing reliance on the existing !important declarations beyond the intentional .d-none case.
  `done_when:` grep -q '@layer utilities' lattice.extra.css returns 0 and the !important on .sr-only properties is the only remaining instance.

- [+] **improvement** | file: `lattice.extra.css` | effort: trivial
  The breakpoint comment block in section 2 lists values in px but the media queries use rem — add a note clarifying the rem-to-px equivalents are at 16px base to avoid confusion for users overriding the root font size.
  `done_when:` The breakpoint comment includes a note such as '(assuming 1rem = 16px default)' next to the px values.

---

## [docs] Docs

### `README.md`

The primary documentation for Lattice CSS, a layout-first CSS utility toolkit. It covers installation, quick start, core concepts, API reference tables, customization tokens, and usage recipes. It describes the relationship between the three distributable files: core, extra, and full bundles.

**Suggestions:**

- [d] **docs** | file: `README.md` | effort: trivial
  The dist/ file tree lists `lattice.full.css` (unminified) but omits `lattice.min.css` counterpart for core — verify the tree matches the actual dist/ contents and add any missing entries (e.g. `lattice.extra.min.css` is listed but `lattice.full.css` lacks a clarifying label distinguishing it from the minified variant).
  `done_when:` The dist/ tree in the README exactly matches the output of `ls dist/` with no omissions or extras.

- [d] **docs** | file: `README.md` | effort: small
  The CDN links use a floating `gh/rlnorthcutt/lattice/dist/` reference without a version tag, meaning consumers always get the latest commit and may receive breaking changes silently. Document a versioned CDN URL pattern (e.g. `@v1.0.0`) and recommend pinning to a release tag.
  `done_when:` README contains at least one example CDN URL with an explicit version tag and a note advising pinning.

- [d] **docs** | file: `README.md` | effort: small
  The Customization section documents CSS custom properties but does not list all available tokens (e.g. gap, spacing scale base, breakpoint overrides if any). Add a complete token reference table so users know the full surface area they can override.
  `done_when:` A table or list in the Customization section enumerates every `--lat-*` variable defined in the source files.

- [d] **docs** | file: `README.md` | effort: trivial
  The Development section says 'To test locally, reference source files in a simple HTML page' but provides no example command or file structure. Add a minimal local-dev instruction (e.g. a one-liner with `npx serve` or a note about the demo HTML if one exists).
  `done_when:` The Development section contains a concrete command or file reference a new contributor can run immediately to preview changes.

---

### `TODO.md`

A plaintext TODO list tracking outstanding bugs and feature work for a CSS/UI framework project. It covers documentation gaps, layout bugs in a grid/lattice system, color adjustments, responsive code snippet issues, card component fixes, and syntax highlighting integration.

**Suggestions:**

- [+] **improvement** | file: `TODO.md` | effort: trivial
  Convert this flat TODO list into a structured format with status tracking (e.g., GitHub Issues, a checklist in Markdown with - [ ] syntax, or a project board) so items can be prioritized and marked done.
  `done_when:` All items use `- [ ]` checkbox syntax or are tracked in a linked issue tracker

- [+] **improvement** | file: `TODO.md` | effort: small
  Add context to each item: which file/component is affected, acceptance criteria, and severity for bugs. The current entries lack enough detail to act on without prior knowledge.
  `done_when:` Each TODO item references a specific file or component and has a one-line acceptance condition

---

### `docs/core.html`

A standalone HTML documentation page for the Lattice Core CSS library, covering installation, design philosophy, and key utility families. It includes usage examples and an embedded stylesheet with light/dark mode support. The page links to sibling docs pages for navigation.

**Suggestions:**

- [~] **refactor** | file: `docs/core.html` | effort: small
  Extract the embedded <style> block into a shared docs stylesheet (e.g. docs/docs.css) to avoid duplicating the same styles across core.html, extra.html, and index.html.
  `done_when:` No <style> tag exists inside core.html and the page visually matches its current appearance after linking to the shared stylesheet.

- [+] **improvement** | file: `docs/core.html` | effort: trivial
  Add a <meta name="description"> tag to improve SEO and link preview quality for the page.
  `done_when:` grep -q '<meta name="description"' docs/core.html returns exit code 0.

- [+] **improvement** | file: `docs/core.html` | effort: trivial
  Mark the current page link in the nav as active (e.g. aria-current="page") for accessibility and wayfinding.
  `done_when:` grep -q 'aria-current="page"' docs/core.html returns exit code 0.

- [d] **docs** | file: `docs/core.html` | effort: medium
  Expand the Key Utility Families table to cover all available utility categories (e.g. color, border, display, overflow) with complete prefix and example columns, or add section anchors for each category so users can link directly to them.
  `done_when:` Visual check confirms the table or anchored sections cover all utility families documented in the source CSS.

- [+] **improvement** | file: `docs/core.html` | effort: small
  The CDN URL in the installation example references an assumed package name (@lattice-css/core) that may not exist on unpkg. Verify the published package name or replace with a real, tested CDN link.
  `done_when:` The CDN href in the code block resolves to a valid CSS file when fetched.

---

### `docs/extra.html`

Documentation page for the Lattice Extra CSS bundle, describing installation options (npm and CDN), use-case guidance, and featured UI patterns like cards and a command palette. It includes inline styles for a clean, readable layout with dark mode support. The page is part of a multi-page documentation site linking to core and extra docs.

**Suggestions:**

- [~] **refactor** | file: `docs/extra.html` | effort: small
  Extract the inline <style> block into a shared docs stylesheet (e.g., docs/docs.css) to avoid duplication across documentation pages and make future style updates easier.
  `done_when:` The <style> block is removed from extra.html and replaced with a <link> to an external stylesheet that produces identical rendering.

- [+] **improvement** | file: `docs/extra.html` | effort: trivial
  Add aria-current="page" to the active nav link ('Lattice Extra Docs') so screen readers can identify the current page.
  `done_when:` The anchor tag for extra.html in the nav contains aria-current="page".

- [+] **improvement** | file: `docs/extra.html` | effort: trivial
  Add a <meta name="description"> tag to improve SEO and social sharing previews for the documentation page.
  `done_when:` A <meta name="description" content="..."> tag is present inside the <head> element.

- [d] **docs** | file: `docs/extra.html` | effort: medium
  Expand the Featured Patterns section to cover additional Extra components mentioned in the introduction (buttons, badges, alerts, state-driven utilities) with code examples, so the docs match the stated feature set.
  `done_when:` Buttons, badges, and alerts each have a dedicated pattern entry with a usage example in the patterns list.

- [!] **security** | file: `docs/extra.html` | effort: trivial
  The CDN link references unpkg.com without a specific version pin or integrity attribute. Add a version pin and a <link integrity="..."> SRI hash to prevent unexpected changes from a compromised CDN.
  `done_when:` The CDN <link> tag includes a specific version in the URL and a valid integrity attribute with a sha384 or sha512 hash.

---

### `docs/index.html`

Landing page for the Lattice.css documentation site, introducing the library's value proposition and core concepts. It showcases the grid system strategy, getting-started instructions with CDN and local bundle options, and live markup examples. Serves as the entry point for users discovering Lattice.css.

**Suggestions:**

- [+] **improvement** | file: `docs/index.html` | effort: trivial
  Add `rel="noopener noreferrer"` to external anchor tags (GitHub links) to prevent reverse tabnapping.
  `done_when:` All `<a>` tags with `href` pointing to external domains include `rel="noopener noreferrer"`.

- [+] **improvement** | file: `docs/index.html` | effort: trivial
  Add an Open Graph meta block (`og:title`, `og:description`, `og:type`) for better social sharing previews.
  `done_when:` At least `og:title`, `og:description`, and `og:type` meta tags are present in `<head>`.

- [+] **improvement** | file: `docs/index.html` | effort: small
  The `<script>` tag already has `type="module"` (which is deferred by default); the redundant `defer` attribute should be removed to avoid confusion.
  `done_when:` `grep -c 'type="module".*defer' docs/index.html` returns 0.

- [d] **docs** | file: `docs/index.html` | effort: small
  The CDN snippet references a `lattice.full.min.css` path but the getting-started prose lists separate Core and Extra bundles — clarify whether a single full bundle or two files should be linked, and update the snippet to match.
  `done_when:` CDN example and explanatory text reference the same consistent set of stylesheet filenames.

- [+] **improvement** | file: `docs/index.html` | effort: trivial
  Add a `<link rel="canonical">` tag pointing to the production URL to prevent duplicate-content indexing issues.
  `done_when:` `grep 'rel="canonical"' docs/index.html` returns a result.

---

