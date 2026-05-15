# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this is

The marketing site for **Natural State Energy** — a brokerage that connects rural electric cooperatives with available power capacity to AI / HPC / data-center operators who need it. The site is deployed to GitHub Pages and served from the apex `naturalstateenergy.com` (see `CNAME`).

The entire production site is a single hand-authored static file: `index.html`. CSS and JS are inlined.

## Layout

```
index.html      # The whole site — markup, <style> block, <script> block
CNAME           # naturalstateenergy.com (GitHub Pages custom domain binding)
```

There is no build step, no package manager, no framework, no router, no asset pipeline. Open `index.html` directly in a browser to preview — that is the dev loop.

## How it's structured (inside index.html)

- `<head>`: title + meta description, Google Fonts preconnect (`Playfair Display`, `DM Sans`), one large `<style>` block that owns the entire design system.
- CSS custom properties at `:root` define the palette (`--green`, `--amber`, `--slate`, `--linen`, `--cream`) and layout (`--max-w: 1100px`, `--px: 48px`). Use these — don't introduce ad-hoc colors.
- Sections in order: `<nav>`, `.hero#home`, `.opp#opportunity`, `.how#how-it-works`, `.why#about`, `.contact#contact`, `<footer>`. Anchor IDs match nav links.
- Contact form is an embedded Jotform iframe (`JotFormIFrame-260982870528064`) plus its UMD embed handler script.
- Inline `<script>` at the bottom handles three things: footer year stamp (`#yr`), mobile nav toggle (`toggleNav()`), and a single `IntersectionObserver` that adds `.in` to `.fade` elements as they scroll into view.

## Design conventions

- Typography: `Playfair Display` for display headings via class names; `DM Sans` for body. The logo wordmark uses `Arial Black` to mimic an etched plate look — keep it.
- Logo is an inline SVG (two-tone triangle, green + amber). It is duplicated in `<nav>` and `<footer>` with different fills/opacities. Edit both when changing geometry.
- Section eyebrows use `.section-label`; section H2s use `.section-h2`. Stay in this rhythm rather than introducing new heading classes.
- Mobile breakpoint is `@media (max-width: 800px)`. The nav collapses to a hamburger; section padding shrinks via `--px: 22px`.
- All fade-in animations rely on `class="fade"` + the IntersectionObserver — adding new content that should animate in just needs the `fade` class.

## Deployment

- GitHub Pages serves `main` from the repository root. Pushing to `main` deploys.
- `CNAME` must remain `naturalstateenergy.com` (one line, no trailing whitespace). GitHub Pages rewrites this file if the custom domain is changed in repo settings — don't fight it.
- There is no preview environment. Branch deploys aren't configured.

## When making changes

- Treat `index.html` as the single source of truth. Do not split CSS/JS into separate files without an explicit request — the simplicity is intentional and there is no bundler to recombine them.
- Preserve the existing palette and section structure unless explicitly asked to redesign. Marketing copy edits should match the existing tone: declarative, short paragraphs, no jargon, no buzzwords.
- The Jotform iframe id and src URL are tied to a specific live form. Don't change them without confirming the new form ID is provisioned.
- Keep the file accessible: every `<a>` with only an icon has `aria-label`, the nav toggle has `aria-label="Menu"`. Maintain that pattern.
- No tests, no linters, no CI. Verify changes by opening `index.html` in a browser and clicking through nav + scrolling through every section on both desktop and mobile widths.

## Out of scope

This repo is not a web app. Don't add frameworks, package.json, build tooling, or static-site generators unless the user explicitly asks for that migration.
