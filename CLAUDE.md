# CLAUDE.md — Jacadi Homepage Sections

## Project Overview

This repository contains reusable **Shopify Liquid theme sections** for a Jacadi homepage. Each file is a self-contained section component that can be added to a Shopify theme via the Theme Editor or Shopify CLI.

**Technology:** Shopify Liquid (HTML + CSS + Liquid templating)
**No build process, no npm, no Node.js.**

---

## Repository Structure

```
jacadi-homepage-sections-/
└── sections/
    ├── featured-collection-grid.liquid   # Product grid from a collection
    ├── hero-slideshow.liquid             # Multi-slide hero banner
    ├── newsletter-banner.liquid          # Email subscription form
    ├── promo-tiles.liquid                # Promotional image tiles
    └── services-icons.liquid             # Services with icons
```

All source files live in `sections/`. There are no subdirectories, build artifacts, or generated files.

---

## Section File Anatomy

Every `.liquid` file follows this three-part structure:

```liquid
<section class="component-name">
  <!-- HTML markup with Liquid templating logic -->
</section>

<style>
  /* Component-scoped CSS (no external stylesheets) */
</style>

{% schema %}
{
  "name": "Section Display Name",
  "settings": [ ... ],
  "blocks": [ ... ],
  "presets": [ ... ]
}
{% endschema %}
```

- **HTML/Liquid block:** The rendered markup. Uses `{{ section.settings.* }}` and `{% for block in section.blocks %}` patterns.
- **`<style>` block:** Inline CSS scoped to this section. Uses Grid and Flexbox; no CSS framework.
- **`{% schema %}`:** JSON configuration that defines the section's settings, blocks, and presets for the Shopify Theme Editor.

---

## Sections Reference

| File | Purpose | Config Type |
|------|---------|-------------|
| `featured-collection-grid.liquid` | Displays 8 products from a chosen collection in a responsive grid | Settings (heading + collection picker) |
| `hero-slideshow.liquid` | Full-width hero banner with multiple slides (image, heading, subheading, CTA button) | Blocks (one block per slide) |
| `newsletter-banner.liquid` | Black-background newsletter signup with email form | Settings (heading, subheading, button text) |
| `promo-tiles.liquid` | Grid of promotional image tiles with links | Blocks (one block per tile) |
| `services-icons.liquid` | Horizontal row of service icons with title and description | Blocks (one block per service) |

---

## Conventions

### Naming
- File names use **kebab-case**: `hero-slideshow.liquid`
- CSS class names match the file/component name: `.hero-slideshow`, `.collection-grid`
- Shopify schema IDs use **camelCase**: `"id": "buttonText"`

### Styling
- All CSS is written inline in `<style>` tags within the section file
- Use CSS Grid (`grid-template-columns: repeat(auto-fit, minmax(...))`) for responsive layouts
- Use Flexbox for single-axis alignment (services row, newsletter form)
- Color palette: `#000` (black), `#fff` (white), `#f7f7f7` (light gray), `#333` (dark text)
- No external CSS frameworks (no Tailwind, Bootstrap, etc.)

### Schema Design
- Use **`settings`** for section-level configuration (applies once to the whole section)
- Use **`blocks`** for repeatable sub-components (slides, tiles, icons)
- Always include a `"presets"` array so the section appears in the Theme Editor "Add section" list

### Liquid Templating
- Access section settings via `{{ section.settings.setting_id }}`
- Iterate blocks with `{% for block in section.blocks %}`
- Apply block attributes with `{{ block.shopify_attributes }}`
- Render images with `{{ image | img_url: 'master' }}`

---

## Development Workflow

### Adding to a Shopify Theme
1. Copy the `.liquid` file into the `sections/` directory of your Shopify theme
2. Deploy via **Shopify CLI**: `shopify theme push`
3. Or upload manually through the **Shopify Admin > Online Store > Themes > Edit code**

### Editing Sections
1. Edit the `.liquid` file directly — no compilation step needed
2. Push to the theme using Shopify CLI or the online code editor
3. Preview in the Theme Editor (`Customize` button in Shopify Admin)

### Creating a New Section
Follow this checklist:
- [ ] Name the file with kebab-case: `sections/my-new-section.liquid`
- [ ] Add an outer `<section class="my-new-section">` wrapper
- [ ] Write styles in a `<style>` block beneath the HTML
- [ ] Add a `{% schema %}` block with `name`, `settings` or `blocks`, and `presets`
- [ ] Test in the Shopify Theme Editor before committing

### No Testing Infrastructure
There are no automated tests. Validation happens visually in the Shopify Theme Editor and storefront preview.

---

## Git Workflow

- **Default branch:** `main`
- **Active development branch:** `claude/add-claude-documentation-Ek7Hf`
- Commit messages should describe the section or change: e.g., `Add hero-slideshow section`, `Fix grid responsive breakpoint in featured-collection-grid`
- Push to `origin` with: `git push -u origin <branch-name>`

---

## AI Assistant Guidelines

When making changes to this repository:

1. **Do not introduce a build system, package.json, or npm dependencies** unless explicitly requested. This is intentionally a pure Liquid/HTML/CSS project.
2. **Keep styles inline** within each section file. Do not create separate CSS files.
3. **Preserve schema structure** — `settings`, `blocks`, and `presets` are required for Shopify Theme Editor compatibility.
4. **One section per file.** Do not combine multiple sections into a single file.
5. **Test mentally against Shopify Liquid syntax** — Liquid has its own filter/tag syntax distinct from Jinja or other template engines.
6. **No JavaScript** unless the feature genuinely requires it (e.g., slideshow interactivity). Keep sections as lightweight as possible.
7. **Respect existing naming conventions** — kebab-case files, matching CSS class names, camelCase schema IDs.
