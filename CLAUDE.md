# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Manifest Movement — a Shopify e-commerce store selling affirmation socks for women. Custom theme built on Shopify's Dawn (v15.4.1) using Liquid, CSS custom properties, and vanilla JS. No build pipeline, no npm.

**Store:** `lumatherapy-2.myshopify.com` (Shopify admin domain; brand domain cannot be changed)
**Theme ID:** `196314202444`
**Repo:** `github.com/Jeramiah-bit/manifest-movement.git`

## Commands

```bash
# Local preview
shopify theme dev --store lumatherapy-2.myshopify.com

# Deploy to live store
shopify theme push --store lumatherapy-2.myshopify.com --theme 196314202444 --allow-live

# List available themes
shopify theme list --store lumatherapy-2.myshopify.com

# Lint Liquid templates
shopify theme check

# Git (always feature branches, never push to main directly)
git checkout -b feat/feature-name
git push origin feat/feature-name
```

Note: `git config http.postBuffer 524288000` is set to handle large asset pushes.

## Architecture

This is a Shopify Online Store 2.0 theme. Key concepts:

- **Templates** (`templates/*.json`) define which sections appear on each page type. `index.json` controls the homepage layout.
- **Sections** (`sections/*.liquid`) are reusable page components with their own schema, settings, and blocks. Section groups (`header-group.json`, `footer-group.json`) control global header/footer.
- **Snippets** (`snippets/*.liquid`) are partials included by sections (e.g., `card-product.liquid` renders every product card site-wide).
- **Config** (`config/settings_data.json`) stores active theme settings (colors, radii, spacing). These are Dawn's global design tokens. `settings_schema.json` defines the available options and their constraints (min/max/step values — check before changing `settings_data.json`).
- **Assets** (`assets/base.css`) is where ALL brand CSS overrides live. Dawn's section-specific CSS files are in `assets/section-*.css` and `assets/component-*.css` — avoid modifying those.

### Brand customizations in `base.css`

All Manifest Movement overrides are in clearly labeled blocks at the top and middle of `base.css`:
- `:root` — Brand tokens (colors, fonts, radii). **Never hardcode hex values anywhere.**
- `/* ── Manifest Movement: Announcement Bar ── */`
- `/* ── Manifest Movement: Header ── */`
- `/* ── Manifest Movement: Image Banner ── */`
- `/* ── Manifest Movement: Product Card ── */`
- `/* ── Manifest Movement: Collection List ── */`
- `/* ── Manifest Movement: Affirmation Quote ── */`
- `/* ── Manifest Movement: Values Multicolumn ── */`
- `/* ── Manifest Movement: Section Headings ── */`
- `/* ── Manifest Movement: Global Buttons ── */`
- `/* ── Manifest Movement: Global Typography ── */`
- `/* ── Manifest Movement: Footer ── */`

### Key customized files

- `snippets/card-product.liquid` — Added affirmation subtitle (product type/vendor) below product title
- `sections/header.liquid` — Custom logo fallback using `manifest-movement-logo.png` from assets
- `layout/theme.liquid` — Google Fonts import (Cormorant Garamond + DM Sans)

## Brand Tokens

```css
--color-brand-plum:      #6B3D7A;   /* Primary — headings, CTAs */
--color-brand-blush:     #F5EEF2;   /* Hero wash, highlights */
--color-brand-rose:      #C4728A;   /* Secondary accent, Add to Cart, hover */
--color-brand-navy:      #2D1B4E;   /* Footer, dark surfaces, body text */
--color-brand-lavender:  #E8DDF0;   /* Light backgrounds, borders */
--color-brand-offwhite:  #FDFBFF;   /* Page & card backgrounds */
--color-brand-gold:      #C9A84C;   /* Premium/limited edition only */

--font-heading:  'Cormorant Garamond', serif;
--font-body:     'DM Sans', sans-serif;
--radius-pill: 40px;  --radius-md: 12px;
```

## Critical Rules

- **Never hardcode colors** — always use `var(--color-brand-*)` tokens
- **Minimal changes** — change only what's needed, don't refactor surrounding code
- **No npm/build pipeline** — vanilla CSS and JS only
- **Research first** — search Shopify docs for current Liquid syntax before implementing
- **Check `settings_schema.json` constraints** before editing `settings_data.json` (e.g., `page_width` steps by 100)
- **Store database overrides local JSON** — section settings in `index.json` only apply as defaults for new installs. If the store has already been customized via the Theme Editor, those settings take precedence
- **Logo** — use `manifest-movement-logo.png` from assets, never recreate in HTML/CSS
- **Feature branches only** — never commit to `main`

## Formatting

- Prettier: 120 char width, single quotes in JS, double quotes in Liquid
- Theme Check: `MatchingTranslations` and `TemplateLength` disabled

## Full Brand Spec

See `/Users/jeramiahseidl/Desktop/Manifest Movement/ManifestMovement_CLAUDE.md` for complete UI specifications (typography table, button specs, component details, security rules, approved integrations).
