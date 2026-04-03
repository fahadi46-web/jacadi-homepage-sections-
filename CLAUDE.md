# CLAUDE.md — Jacadi Homepage Sections

This file provides guidance for AI assistants working in this repository.

## Repository Overview

This is a **Shopify theme sections library** for the Jacadi brand homepage. It contains five reusable Liquid section components that can be installed into any Shopify theme and configured through the Shopify theme editor UI.

**No build system, no package manager, no test suite.** Every file is a self-contained Shopify section — drop it into a theme's `sections/` directory and it works.

---

## Directory Structure

```
jacadi-homepage-sections-/
├── CLAUDE.md                          # This file
└── sections/                          # All Shopify section components
    ├── hero-slideshow.liquid          # Animated hero banner with slides
    ├── featured-collection-grid.liquid# Product grid from a collection
    ├── promo-tiles.liquid             # Marketing tiles with images & links
    ├── newsletter-banner.liquid       # Email signup form
    └── services-icons.liquid          # Service icons with descriptions
```

---

## Section File Anatomy

Every `.liquid` file follows a strict three-part structure — **in this order**:

```
1. HTML/Liquid markup
2. <style> block (scoped CSS)
3. {% schema %} block (JSON schema for theme editor)
```

### 1. HTML/Liquid Markup

- Use `section.settings.<id>` to access **section-level** settings.
- Use `section.blocks` with a `{% for block in section.blocks %}` loop for **repeatable block items**.
- Access block-level settings with `block.settings.<id>`.
- Images use the `| image_url` Liquid filter (Shopify CDN).
- Prices use the `| money` Liquid filter.
- Customer forms use `{% form 'customer' %}...{% endform %}`.

### 2. Style Block

- Inline `<style>` tags are embedded directly in the section file — no external CSS.
- CSS classes use **kebab-case** and match the section's semantic name (e.g., `.hero-slideshow`, `.hero-slide`, `.hero-content`).
- Minimal whitespace in CSS property declarations (e.g., `display:grid;` not `display: grid;`).
- Layout uses CSS Grid (`grid-template-columns: repeat(auto-fit,minmax(...,1fr))`) or Flexbox.
- Images within sections use `width:100%` to fill their container.

### 3. Schema Block

- Must be valid JSON wrapped in `{% schema %}...{% endschema %}`.
- Every schema includes `"name"` and `"presets"` (with at least one preset matching the name).
- Settings use Shopify's input types: `"text"`, `"image_picker"`, `"url"`, `"collection"`.
- Block-based sections define a `"blocks"` array; section-wide settings go in `"settings"`.
- Setting objects always have `"type"`, `"id"`, and `"label"` — in that order.
- IDs are **snake_case** (e.g., `"id": "button_label"`).

---

## The Five Sections

| File | Schema Name | Type | Settings | Blocks |
|---|---|---|---|---|
| `hero-slideshow.liquid` | Hero Slideshow | Block-based | — | slide: image, heading, subheading, link, button_label |
| `featured-collection-grid.liquid` | Featured Collection Grid | Settings-based | heading, collection | — |
| `promo-tiles.liquid` | Promo Tiles | Block-based | — | tile: image, title, link |
| `newsletter-banner.liquid` | Newsletter Banner | Settings-based | heading | — |
| `services-icons.liquid` | Services Icons | Block-based | — | service: icon, text |

---

## Coding Conventions

### Liquid
- Use `{{ }}` for output, `{% %}` for logic/tags.
- Always apply `| image_url` when rendering images from settings.
- Use `limit:8` on collection product loops to cap output.
- No Liquid comments in files — keep markup clean.

### CSS
- All styles scoped to this section live inside one `<style>` block at the bottom of the markup, above `{% schema %}`.
- Responsive layouts use `repeat(auto-fit, minmax(...px, 1fr))` — no media queries.
- Brand color constants observed: black (`#000`), white (`#fff`), light grey (`#f7f7f7`).
- Button style: `padding:12px 25px; background:#000; color:#fff; text-decoration:none;`.
- Hero overlay: `background: rgba(255,255,255,0.85); padding:30px; text-align:center;`.

### Schema JSON
- Compact inline format for simple setting objects: `{ "type": "text", "id": "heading", "label": "Heading" }`.
- Presets array always has exactly one entry whose `"name"` matches the schema `"name"`.

---

## Development Workflow

### Adding a New Section

1. Create a new file in `sections/` named `<section-name>.liquid`.
2. Write HTML/Liquid markup using existing sections as reference.
3. Add a `<style>` block with scoped CSS.
4. Add a `{% schema %}` block with a valid JSON schema.
5. Validate schema JSON before committing (must be valid JSON).

### Installing into a Shopify Theme

Copy the desired `.liquid` file(s) into the `sections/` directory of a Shopify theme (e.g., a Dawn-based theme). The sections immediately appear in the **Add section** panel of the theme editor.

### No Build Step

There is no compilation, transpilation, or bundling. Files are used as-is by Shopify's Liquid rendering engine.

---

## Shopify Schema Reference

### Supported Setting Input Types (used in this repo)

| Type | Usage |
|---|---|
| `text` | Single-line text string |
| `image_picker` | Returns an image object; pipe through `\| image_url` |
| `url` | URL string for links |
| `collection` | Returns a collection handle for `collections[handle].products` |

### Block Structure

```json
{
  "type": "unique_type_id",
  "name": "Display Name",
  "settings": [
    { "type": "text", "id": "setting_id", "label": "Label" }
  ]
}
```

### Preset Structure

```json
"presets": [{ "name": "Section Display Name" }]
```

---

## Git Conventions

- Branch naming: `claude/<purpose>-<id>` for AI-generated branches.
- Commit messages describe the file operation (e.g., `"Update and rename X to Y"`).
- Main branch: `main`. Development happens on feature branches, merged to `main`.
- Remote: `origin` at `http://local_proxy@127.0.0.1:36889/git/fahadi46-web/jacadi-homepage-sections-`.

---

## What NOT to Do

- Do not add a `package.json`, build scripts, or JS framework tooling — this is pure Shopify Liquid.
- Do not add JavaScript to sections unless the feature explicitly requires client-side interactivity.
- Do not use external CSS frameworks (Bootstrap, Tailwind) — keep sections self-contained.
- Do not add media queries for responsiveness — use CSS Grid `auto-fit`/`minmax` instead.
- Do not create additional subdirectories — all sections live flat in `sections/`.
- Do not modify schema `"id"` values of existing settings — this would break saved theme editor data.
