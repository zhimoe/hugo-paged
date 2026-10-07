# Paged theme maintenance plan

The current rendered site is the visual baseline. Refactors in this plan must
preserve that baseline at mobile and desktop breakpoints unless a documented
bug is being fixed.

## Planned commits

1. Consolidate `static/css/custom.css` into `static/css/style.css`.
   - Keep the current warm paper colors, texture layers, crop marks, spacing,
     shadows, and light-only color scheme.
   - Replace the old declarations that are currently overridden: default color
     variables, dark-mode inversion, crop-mark borders/mask, and desktop paper
     margins.
   - Load only `style.css` and `fonts.css` after the merge.
2. Remove the obsolete third crop-mark mask.
   - The current crop marks are drawn by `.crop-h` and `.crop-v`; `.crop-c` is
     hidden and no longer contributes to rendering.
   - Remove its HTML, selector references, and outdated example documentation.
3. Remove dead legacy CSS without changing live rendering.
   - Delete commented-out rules and variables that have no active consumers.
   - Remove `.toc-list` rules left behind after the old list layout was deleted.
   - Keep selectors created at runtime by the theme's JavaScript utilities.
4. Fix responsive content overflow.
   - Allow otherwise unbreakable prose/list/table-cell content to wrap.
   - Constrain tables to the available width.
   - This intentionally changes only pages that currently overflow their paper
     or viewport; code blocks retain horizontal scrolling.
5. Keep analytics markup inside a valid document structure.
   - Move the optional Cloudflare page-view script inside `body` without
     changing its execution or rendered output.
6. Make theme-owned asset links work below a subpath.
   - Generate favicon, touch-icon, and AI-overview URLs through Hugo instead of
     hard-coding root-relative paths.
7. Move the project's search-page style override into the theme.
   - Scope the rules to `.search` so other pages are unaffected.
   - Remove the matching inline `<style>` block from `content/search.md` once
     the parent project points at the updated theme commit.

## Verification

- Run a production Hugo build with warnings treated as failures.
- Check every generated sitemap page for horizontal document overflow at 320,
  375, 767, 768, 991, 992, and 1440 pixel viewport widths.
- Compare representative home, article, archive, taxonomy, search, and 404 pages
  before and after the CSS-only refactors.
- Verify both light and dark OS preferences still render the same daylight paper
  theme.

## Intentionally retained

- `fonts.css` remains separate because it has a single, distinct responsibility.
- Empty `head`/`foot`/`main` extension partials remain supported customization
  points.
- Copy-button, heading-anchor, keyboard, full-width, and TOC selectors remain
  because their elements or classes are added conditionally at build time or by
  runtime scripts.
