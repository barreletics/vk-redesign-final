# VK Shopify Handoff — Hammer

**Start here:** [HAMMER-HANDOFF.md](./HAMMER-HANDOFF.md)
**Data model note:** [BADGES-AND-DATA.md](./BADGES-AND-DATA.md)

This folder contains 5 exemplar Online Store 2.0 section files that map the VK design system to the patterns Hammer will use when building the new Vintage King theme on Horizon.

## Files

```
shopify-handoff/
├── HAMMER-HANDOFF.md            ← read first
├── BADGES-AND-DATA.md           ← metafield + tag strategy
├── sections/
│   ├── vk-hero.liquid               (H4 · H5 · H8)
│   ├── vk-value-strip.liquid        (V7 · V7b · V8 · V12)
│   ├── vk-editorial-grid.liquid     (B-B · B-B2 · optional B-P band)
│   ├── vk-collection-filters.liquid (P-A — UI shell only)
│   └── vk-mega-menu.liquid          (Mega Menu Lab — structure shell)
└── snippets/
    └── vk-hero-ctas.liquid          (shared by vk-hero.liquid)
```

## Canonical references

- Design system + page index: <https://barreletics.github.io/vk-redesign-final/index-v2.html#pages>
- Mega menu prototype: <https://barreletics.github.io/vintage-king-redesign/mega-menu/VintageKing-MegaMenu-CursorLab-v2.html>
- Broader migration brief: `../../vk-secondary-pages/AGENCY-BRIEF.md`

## Locked design tokens

| Token | Value | Use |
|---|---|---|
| `--vk-near-black` | `#1A1A18` | Body text, dark surfaces |
| `--vk-mid-grey` | `#6B6B68` | Secondary text |
| `--vk-red` | `#C0392B` | Core ecommerce CTAs, nav chrome |
| `--vk-amber` | `#D4860A` | Secondary / program page accents |
| `--vk-amber-bg` | `#F5E6C8` | Credential bar wash (V7) |
| `--vk-warm-grey` | `#E8E3DC` | Stats strip background (V7b) |
| `--vk-off-white` | `#FAF9F6` | Filter panel background |

## Usage at a glance

| Do | Do not |
|---|---|
| Copy tokens, class naming, and layout rhythm into Horizon | Fork our static prototype HTML line-by-line into Liquid |
| Use one section per content type with **presets** for layout | Build 56 separate sections (one per index letter code) |
| Open live prototype URLs while building | Treat these exemplars as drop-in production code |
| Wire filters via Shopify Search & Discovery | Hardcode chip data in the section |
