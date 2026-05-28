# VK Redesign — Hammer Handoff

A small package to help Hammer build the new Vintage King Shopify site on **Horizon**, migrating off Magento.

This is **not a theme**. It is:
- a link to the **design spec** (the single source of truth)
- **5 Liquid section files** that show the pattern we want for the most common page parts
- a short note on how product **badges** and other dynamic data should be wired

---

## 1) The design spec (the "what")

The marketing team has already picked the approved page layouts. Everything lives here:

**Design system + page index:**
[https://barreletics.github.io/vk-redesign-final/index-v2.html#pages](https://barreletics.github.io/vk-redesign-final/index-v2.html#pages)

Inside that index:
- Scroll up from `#pages` for the component library (Heroes, Strips, Body sections, PLP, Footer).
- Each component has a code like **H4**, **H5**, **V7b**, **B-B**, **P-A**. Those codes are referenced in every Liquid file we ship.
- The `#pages` table lists every built prototype page. Click any **"Built"** name to open it live.

**Mega menu reference (separate prototype):**
[https://barreletics.github.io/vintage-king-redesign/mega-menu/VintageKing-MegaMenu-CursorLab-v2.html](https://barreletics.github.io/vintage-king-redesign/mega-menu/VintageKing-MegaMenu-CursorLab-v2.html)

> Treat the prototypes as the **visual + behavioral bible**. CSS/JS in the prototypes is inlined for demo purposes and is not meant to be pasted into the theme verbatim — use the structure and styles as the spec, and re-implement inside the Horizon theme conventions.

---

## 2) The 5 Liquid samples (the "how")

These files live in [`sections/`](./sections/) and [`snippets/`](./snippets/). Each is a working Online Store 2.0 section with theme-editor controls and the exact CSS from the prototype.

| File | Covers (index codes) | What it demonstrates |
|------|----------------------|----------------------|
| [`sections/vk-hero.liquid`](./sections/vk-hero.liquid) | **H4 · H5 · H8** | One section, three layout presets via a `layout` select. Eyebrow, rich-text headline, lede, image, two CTAs, accent color toggle. |
| [`sections/vk-value-strip.liquid`](./sections/vk-value-strip.liquid) | **V7 · V7b · V8 · V12** | Repeater blocks (max 4) with a preset select. Same data, four visual treatments. |
| [`sections/vk-editorial-grid.liquid`](./sections/vk-editorial-grid.liquid) | **B-B · B-B2** + optional B-P band | Repeater cards (max 4) with two grid presets and an optional full-bleed image band on top. |
| [`sections/vk-collection-filters.liquid`](./sections/vk-collection-filters.liquid) | **P-A** | **UI shell only.** Pill row, quick-link row, meta row, expandable facet panel. See note below about wiring real filters. |
| [`sections/vk-mega-menu.liquid`](./sections/vk-mega-menu.liquid) | Mega Menu Lab | **Structure shell.** Header bar, panel grid, featured rail, promo tiles. All link data comes from a `linklist` setting. |

Plus one shared snippet:
- [`snippets/vk-hero-ctas.liquid`](./snippets/vk-hero-ctas.liquid) — shared CTA block used by `vk-hero.liquid`.

### What these samples **are**
- Pixel-faithful to the prototypes (colors, fonts, spacing, breakpoints all match).
- Standard Shopify 2.0 schema patterns: `presets`, `settings`, `blocks`, `block.settings`, `section.shopify_attributes`.
- Drop-in friendly: copy a file into `theme/sections/`, add the section in the theme editor, and it renders.

### What these samples **are not**
- Not connected to cart, checkout, customer accounts, search, recommendations, or recently-viewed.
- Not a substitute for **Shopify Search & Discovery** — the filter section is a UI shell. See section 3.
- Not the full set of components in the index. They are exemplars Hammer can clone and extend.

### Hammer owns (the production work)
- Horizon theme integration: tokens, base layout, header/footer chrome, performance.
- Wiring Shopify Search & Discovery (Storefront Filtering API) into the filter shell.
- Cart/drawer behavior, checkout customization, customer accounts.
- PDP template (we have prototypes — the section file is being saved for a later pass).
- Responsive/mobile parity beyond the breakpoints we shipped.
- Real product, collection, and metafield data (see section 3).

---

## 3) Dynamic data (badges, conditions, "Just Landed", etc.)

You will see badges throughout the prototypes — **New**, **Used**, **Vintage**, **Open Box**, **Co-op**, **Just Landed**, **Tube**, etc. These are **driven by product data**, not by section settings. The section files render the look; the product record supplies the value.

Recommended data model:

| Badge / facet | Source on the product | Notes |
|---|---|---|
| **Condition** (`New` / `Used` / `Vintage` / `Open Box` / `B-Stock`) | Metafield: `vk.condition` (single-line text or metaobject reference) | Drives the badge color + sort/filter facets. |
| **Co-op** | Tag `co-op` *or* metafield `vk.is_co_op` (boolean) | Used in PLP "Quick" row and the mega-menu featured rail. |
| **Just Landed** | Tag `just-landed` *or* metafield `vk.just_landed_until` (date) | Auto-expires on the merchant-set date. |
| **Tube / Ribbon / Condenser** (mic type) | Product metafield + Search & Discovery facet | Powers the canonical filter pills (P-A). |
| **Era / Year** (vintage) | Metafield `vk.year` (number) | Used on vintage PDP + Hall of Fame. |
| **Specialist** (assigned consultant) | Metafield `vk.specialist` (metaobject reference) | Used on PDP and Audio Consultants page. |

For the full strategy and example Liquid snippets, see [`BADGES-AND-DATA.md`](./BADGES-AND-DATA.md).

---

## 4) Suggested next steps

1. Open the design index and skim the `#pages` table to understand the page set.
2. Open the 5 Liquid files and confirm the patterns map cleanly to your Horizon conventions.
3. Tell us what's missing or what you'd want adjusted before we extend the sample set (PDP, footer, sliding cart, etc.).
4. Schedule a 30-min walk-through if helpful — we can demo each prototype live.

— Andrew (Barreletics, on behalf of VK)
