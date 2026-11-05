# Design Spec: Documentation Architecture for Math On Canvas

**Date:** 2026-05-06  
**Last updated:** 2026-11-04  
**Status:** Approved

## Goal

Build a documentation site for the Math Poster Editor (math-on-canvas) that:
1. Helps users learn the editor
2. Drives SEO traffic
3. Supports EN and RU from day one, with DE and ES ready to add later
4. Integrates YouTube tutorial videos inline on relevant guide pages

---

## i18n Architecture

**Approach:** EN as root locale (no URL prefix), RU at `/ru/`. Starlight fallback shows EN content for any page not yet translated, with a built-in "this page is not yet translated" banner.

**Starlight config:**
```js
locales: {
  root: { label: 'English', lang: 'en' },
  ru:   { label: 'Русский', lang: 'ru' },
  de:   { label: 'Deutsch', lang: 'de' },
  es:   { label: 'Español', lang: 'es' },
},
defaultLocale: 'root',
```

DE and ES are declared in config from day one so translations can be added without restructuring.

**File structure:**
```
src/content/docs/
├── index.mdx                    ← EN root homepage
├── getting-started/
│   ├── introduction.md
│   ├── quick-start.mdx          ← .mdx for video embed
│   └── interface.md
├── guides/
│   ├── formulas.md
│   ├── symbols.md
│   ├── shapes-2d/
│   │   ├── index.md
│   │   ├── circle.mdx           ← .mdx for video embed
│   │   ├── rectangle.mdx        ← .mdx for video embed
│   │   ├── triangle.mdx         ← .mdx for video embed
│   │   ├── polygon.md
│   │   └── line-arrow.mdx       ← .mdx for video embed
│   ├── shapes-3d/
│   │   ├── index.md
│   │   ├── cube.md
│   │   ├── sphere.md
│   │   ├── cone.md
│   │   ├── cylinder.md
│   │   ├── prism.md
│   │   └── pyramid.md
│   ├── text.mdx                 ← .mdx for video embed
│   ├── drawing.md
│   ├── images.md
│   ├── annotations.md
│   ├── connections.md
│   ├── pages-documents.md
│   ├── ai-assistant.md
│   └── export.md
├── reference/
│   ├── keyboard-shortcuts.md
│   ├── element-properties.md
│   └── export-formats.md
└── ru/
    ├── index.mdx
    ├── getting-started/
    │   ├── introduction.md
    │   ├── quick-start.mdx
    │   └── interface.md
    └── guides/
        ├── formulas.md
        └── (others added progressively)
```

Pages needing MDX (`.mdx`) are those with YouTube video embeds, plus the homepages (`index.mdx`, `ru/index.mdx`), which use the `splash` template. All others use plain Markdown (`.md`).

**RU translation priority (first iteration, all translated):**
1. `index.mdx` — homepage
2. `getting-started/introduction.md`
3. `getting-started/quick-start.mdx`
4. `getting-started/interface.md`
5. `guides/formulas.md`

Reference section (`reference/`) starts EN-only. Translated on demand.

---

## Content Structure

### Getting Started
Task-based onboarding. Translated to RU in first iteration.

| Page | Purpose |
|---|---|
| Introduction | What the editor is, who it's for, guest vs authenticated features table |
| Quick Start | Create first poster in 5 minutes — step-by-step with screenshots + YouTube video embed |
| Interface Overview | UI layout (Toolbar, EditorBar, Canvas, Sidebar, Palettes) with annotated diagram |

### Guides
One page per feature/element type. Mixed approach: short task-oriented intro + full property reference.

| Page | Key content | YouTube embed |
|---|---|---|
| Formulas | LaTeX editor, symbol palette (150+ symbols), predefined formulas (60+ entries by category) | — |
| Mathematical Symbols | Symbol palette: 150+ symbols by category | — |
| 2D Shapes (overview) | All 14 kinds, common stroke/fill/dash properties | — |
| Circle, Rectangle, Triangle, Line & Arrow | One page per shape: inserting, properties | yes (one video per page) |
| Polygon & Polyline | Inserting, properties | — |
| 3D Shapes (overview) | All 7 kinds, dimensions, rotation, vertex labels, face opacity | — |
| Cube, Sphere, Cone, Cylinder, Prism, Pyramid | One page per shape | — |
| Text | Font family/size/color, bold/italic, fixed width | yes |
| Drawing | Pencil vs Marker, stroke settings | — |
| Images | Upload, resize, aspect-ratio lock | — |
| Annotations | Angle annotations (arc/square/dot), side annotations | — |
| Connections & Vertices | Enable vertex mode, snap tolerance, connection propagation | — |
| Pages & Documents | Page formats (A4/Letter/Poster), portrait/landscape, multi-page operations | — |
| AI Assistant | Formula tab, Document tab, job lifecycle, auth requirement | — |
| Export | PDF/SVG/PNG, page selection dialog, cloud export (auth only), multi-page ZIP | — |

### Reference
EN-only at launch. Translated on demand.

| Page | Content |
|---|---|
| Keyboard Shortcuts | Arrow-key nudge, Delete, Undo/Redo, Escape |
| Element Properties | Full table of all properties per element type |
| Supported Formats | PDF/SVG/PNG specs, font embedding, ZIP for multi-page |

---

## YouTube Video Integration

Videos are embedded inline on the relevant guide page using an MDX iframe. No separate Tutorials section.

**Embed pattern** (in `.mdx` files):

```mdx
<iframe
  width="100%"
  style="aspect-ratio: 16/9; border: none; border-radius: 8px;"
  src="https://www.youtube.com/embed/VIDEO_ID"
  allowfullscreen
/>
```

**Current placement (6 videos):**
- Quick Start video → `/getting-started/quick-start` (also `/ru/getting-started/quick-start`)
- "How to draw [element]" videos → the guide page of that element: `/guides/shapes-2d/circle`, `/guides/shapes-2d/rectangle`, `/guides/shapes-2d/triangle`, `/guides/shapes-2d/line-arrow`, `/guides/text`

The embed goes directly after the page's intro paragraph.

**RU pages with video embeds** use the same YouTube embed (videos are in English with Russian subtitles). Add a note below the embed, in Russian: `> Видео на английском языке. Субтитры на русском доступны — нажмите CC в плеере.`

If future videos don't map to an existing guide page, a `tutorials/` section is added at that point.

---

## SEO Strategy

### Frontmatter per page

```yaml
---
title: LaTeX Formula Editor — Math On Canvas
description: Add and edit math formulas using LaTeX. Live preview, 
  150+ symbols, 60+ predefined formulas for algebra and geometry.
---
```

- `title`: `[Feature keyword] — Math On Canvas` (60 chars max)
- `description`: 120–160 chars, concrete facts (numbers, formats), no filler
- Homepages are the exception: their titles are `Math Poster Editor Documentation` / `Документация Math On Canvas`

### Automatic from Astro/Starlight
- `sitemap.xml` covering all pages in all locales
- `hreflang` cross-links between all locale versions (EN, RU, DE, ES)
- Canonical URLs per locale

### Target queries per section

| Section | Primary queries |
|---|---|
| Introduction | math poster editor, online math diagram creator |
| Quick Start | how to use math on canvas, math diagram editor tutorial |
| Formulas | LaTeX formula editor online, math formula canvas editor |
| 2D Shapes | draw geometric shapes online, math diagram shapes |
| 3D Shapes | 3D math diagram online, draw 3D shapes browser |
| Export | export math diagram PDF, save math poster SVG PNG |
| AI Assistant | AI math formula generator, generate math poster AI |

### Content writing rules
- First `h1` on every page repeats the primary keyword from `title` (homepages use the `splash` hero instead)
- `h2`/`h3` headings build the TOC — keep them descriptive and keyword-rich
- Each guide page ends with a **Related** links block pointing to 2–3 related pages

---

## Sidebar Configuration

The sidebar is defined explicitly in `astro.config.mjs`. Each item has an English `label` and `translations` for RU, DE and ES, so labels are localized even on pages that fall back to EN content. They do not come from the page `title`.

```
Open Editor ↗ (external link to the editor)

Getting Started
  Introduction
  Quick Start
  Interface Overview

Guides
  Formulas
  Mathematical Symbols
  2D Shapes
    Overview
    Circle
    Rectangle
    Triangle
    Polygon & Polyline
    Line & Arrow
  3D Shapes
    Overview
    Cube
    Sphere
    Cone
    Cylinder
    Prism
    Pyramid
  Text
  Drawing
  Images
  Annotations
  Connections & Vertices
  Pages & Documents
  AI Assistant
  Export

Reference
  Keyboard Shortcuts
  Element Properties
  Supported Formats
```

---

## Hosting

- Production URL: `https://docs.math-on-canvas.com` (`site` in `astro.config.mjs`)
- Deployed on Vercel. `vercel.json` permanently redirects old `/docs` and `/docs/*` URLs to the site root

---

## Out of Scope

- Writing the actual page content (separate ongoing effort)
- DE and ES content (added later, infrastructure already in place)
- Custom Starlight components beyond ThemeSelect already overridden
- Search customization (Starlight's built-in Pagefind is sufficient)
