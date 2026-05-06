# Design Spec: Documentation Architecture for Math On Canvas

**Date:** 2026-05-06  
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
│   ├── formulas.mdx
│   ├── shapes-2d.mdx
│   ├── shapes-3d.mdx
│   ├── text.md
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
        ├── formulas.mdx
        └── (others added progressively)
```

Pages needing MDX (`.mdx`) are those with YouTube video embeds. All others use plain Markdown (`.md`).

**RU translation priority (first iteration):**
1. `index.mdx` — homepage
2. `getting-started/introduction.md`
3. `getting-started/quick-start.mdx`
4. `getting-started/interface.md`
5. `guides/formulas.mdx`

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
| Formulas | LaTeX editor, symbol palette (150+ symbols), predefined formulas (60+ entries by category) | if available |
| 2D Shapes | All 14 kinds with examples, stroke/fill/dash properties | if available |
| 3D Shapes | All 7 kinds, dimensions, rotation, vertex labels, face opacity | if available |
| Text | Font family/size/color, bold/italic, fixed width | — |
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

**Current 5 videos placement:**
- Quick Start video → `/getting-started/quick-start`
- "How to draw [element]" videos → corresponding guide page (2D Shapes, 3D Shapes, etc.)

**RU pages with video embeds** use the same YouTube embed (videos are in English with Russian subtitles). Add a note: `> Russian subtitles are available — click CC in the video player.`

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

### Automatic from Astro/Starlight
- `sitemap.xml` covering all pages in all locales
- `hreflang` cross-links between EN and RU versions
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
- First `h1` on every page repeats the primary keyword from `title`
- `h2`/`h3` headings build the TOC — keep them descriptive and keyword-rich
- Each guide page ends with a **Related** links block pointing to 2–3 related pages

---

## Sidebar Configuration

Sidebar labels are i18n-aware: Starlight uses the page `title` frontmatter as the sidebar label per locale automatically when content exists in that locale's directory.

```
Getting Started
  Introduction
  Quick Start
  Interface Overview

Guides
  Formulas
  Mathematical Symbols
  2D Shapes
    Circle
    Rectangle
    Triangle
    Polygon
    Line & Arrow
  3D Shapes
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

## Out of Scope

- Writing the actual page content (separate ongoing effort)
- DE and ES content (added later, infrastructure already in place)
- Custom Starlight components beyond ThemeSelect already overridden
- Search customization (Starlight's built-in Pagefind is sufficient)
