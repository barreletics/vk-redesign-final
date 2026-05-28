# Badges and Dynamic Data — Strategy Note

The prototypes show a lot of small visual signals on product cards and PDPs: **New**, **Used**, **Vintage**, **Open Box**, **Co-op**, **Just Landed**, **Tube**, **Condenser**, etc.

These are **not hardcoded in the section files**. They render off the product record. The section gives the visual; the product supplies the truth.

This note explains the recommended model so the design across PLP, PDP, the mega-menu featured rail, and the homepage "Rare Gear" and "Short List" sections all stay in sync from one source of data.

---

## TL;DR

- **Conditions and structured attributes → product metafields.** They are filterable, sortable, validated, and easy to manage at scale.
- **Light marketing flags (co-op, just-landed, staff-pick) → tags or boolean metafields.** Quick to toggle on individual products.
- **Connected lists (specialists, brand pages, collections) → metaobject references.** One source of truth, reusable across templates.

---

## Recommended metafield definitions

Set these up in Shopify admin under **Settings → Custom data → Products**. All use the `vk` namespace so they group together.

| Key | Type | Used for | Filterable? |
|---|---|---|---|
| `vk.condition` | Single-line text *or* metaobject ref | Badge text + color (`New`, `Used`, `Vintage`, `Open Box`, `B-Stock`) | ✅ (expose in Search & Discovery) |
| `vk.year` | Number (integer) | Vintage year on PDP, Hall of Fame timeline | ✅ |
| `vk.era` | Single-line text | "1960s", "1970s", "Modern" filter chip | ✅ |
| `vk.is_co_op` | Boolean | Co-op badge, "Co-op" quick filter | ✅ |
| `vk.just_landed_until` | Date | Auto-expiring "Just Landed" badge | ✅ (computed via date filter) |
| `vk.staff_pick` | Boolean | "Staff Pick" badge on PLP | ✅ |
| `vk.specialist` | Metaobject reference (`vk_specialist`) | PDP consultant card, Audio Consultants page | – |
| `vk.included_items` | Multi-line text *or* rich text | "Comes with" block on PDP | – |

> If the team prefers tags for speed, the equivalents are:
> `co-op`, `just-landed`, `staff-pick`, `condition::new`, `condition::used`, `condition::vintage`, `condition::open-box`. Use one or the other — not both — so badge logic has a single source.

---

## How a section file uses this

The pattern in `vk-collection-filters.liquid` is intentionally a UI shell. To wire real facets:

```liquid
{%- for filter in collection.filters -%}
  {%- if filter.label == 'Condition' -%}
    {%- for value in filter.values -%}
      <button class="vk-flt__chip {% if value.active %}vk-flt__chip--red{% endif %}">
        {{ value.label }} ({{ value.count }})
      </button>
    {%- endfor -%}
  {%- endif -%}
{%- endfor -%}
```

A typical product-card badge looks like this. Use a single snippet `vk-product-badges.liquid` and call it from every card template (PLP card, mega-menu featured, "Rare Gear" tile, etc.) so the rules live in one place:

```liquid
{%- comment -%} snippets/vk-product-badges.liquid {%- endcomment -%}
{%- liquid
  assign cond = product.metafields.vk.condition.value | default: ''
  assign is_coop = product.metafields.vk.is_co_op.value
  assign landed_until = product.metafields.vk.just_landed_until.value
  assign now = 'now' | date: '%s' | times: 1
  assign landed_ts = 0
  if landed_until != blank
    assign landed_ts = landed_until | date: '%s' | times: 1
  endif
-%}
<div class="vk-badges">
  {%- if cond != '' -%}
    <span class="vk-badge vk-badge--{{ cond | handleize }}">{{ cond }}</span>
  {%- endif -%}
  {%- if is_coop -%}
    <span class="vk-badge vk-badge--coop">Co-op</span>
  {%- endif -%}
  {%- if landed_ts > now -%}
    <span class="vk-badge vk-badge--landed">Just Landed</span>
  {%- endif -%}
</div>
```

The CSS for the badge colors lives in the theme's shared CSS bundle, not inside any section.

---

## Why this matters for the redesign

Once the data model is in place:
- Merchandisers can flip badges on/off in the admin without engineering.
- The same condition value drives **filtering**, **badging**, **sorting**, and **search facets** — no drift.
- Hammer can render PLP, PDP, mega-menu featured, and homepage cards from one shared partial.
- New surfaces (newsletter blocks, dynamic landing pages, search results) inherit the same look automatically.

This is the foundation that lets the visual system in the prototypes scale across a real catalog.
