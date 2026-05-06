# Documentation Infrastructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Set up the full documentation infrastructure for Math On Canvas: i18n config (EN root + RU/DE/ES), complete sidebar, and scaffolded page files (frontmatter + heading skeleton) for all sections — Getting Started, Guides, Reference, and RU priority pages.

**Architecture:** Astro/Starlight with EN as root locale (no URL prefix) and RU at `/ru/`. All 34 content files are created as stubs with SEO-optimised frontmatter and h2 section structure. Actual prose content is written separately. YouTube video embeds are marked with `YOUTUBE_VIDEO_ID` placeholders — the project owner must supply the real IDs before publishing.

**Tech Stack:** Astro 6, Starlight 0.38, Markdown, MDX

---

## File Map

| File | Action |
|---|---|
| `astro.config.mjs` | Modify — add locales + full sidebar |
| `src/content/docs/guides/example.md` | Delete |
| `src/content/docs/reference/example.md` | Delete |
| `src/content/docs/index.mdx` | Rewrite — Math On Canvas homepage |
| `src/content/docs/getting-started/introduction.md` | Create |
| `src/content/docs/getting-started/quick-start.mdx` | Create |
| `src/content/docs/getting-started/interface.md` | Create |
| `src/content/docs/guides/formulas.mdx` | Create |
| `src/content/docs/guides/symbols.md` | Create |
| `src/content/docs/guides/text.md` | Create |
| `src/content/docs/guides/drawing.md` | Create |
| `src/content/docs/guides/images.md` | Create |
| `src/content/docs/guides/annotations.md` | Create |
| `src/content/docs/guides/connections.md` | Create |
| `src/content/docs/guides/pages-documents.md` | Create |
| `src/content/docs/guides/ai-assistant.md` | Create |
| `src/content/docs/guides/export.md` | Create |
| `src/content/docs/guides/shapes-2d/index.md` | Create |
| `src/content/docs/guides/shapes-2d/circle.md` | Create |
| `src/content/docs/guides/shapes-2d/rectangle.md` | Create |
| `src/content/docs/guides/shapes-2d/triangle.md` | Create |
| `src/content/docs/guides/shapes-2d/polygon.md` | Create |
| `src/content/docs/guides/shapes-2d/line-arrow.md` | Create |
| `src/content/docs/guides/shapes-3d/index.md` | Create |
| `src/content/docs/guides/shapes-3d/cube.md` | Create |
| `src/content/docs/guides/shapes-3d/sphere.md` | Create |
| `src/content/docs/guides/shapes-3d/cone.md` | Create |
| `src/content/docs/guides/shapes-3d/cylinder.md` | Create |
| `src/content/docs/guides/shapes-3d/prism.md` | Create |
| `src/content/docs/guides/shapes-3d/pyramid.md` | Create |
| `src/content/docs/reference/keyboard-shortcuts.md` | Create |
| `src/content/docs/reference/element-properties.md` | Create |
| `src/content/docs/reference/export-formats.md` | Create |
| `src/content/docs/ru/index.mdx` | Create |
| `src/content/docs/ru/getting-started/introduction.md` | Create |
| `src/content/docs/ru/getting-started/quick-start.mdx` | Create |
| `src/content/docs/ru/getting-started/interface.md` | Create |
| `src/content/docs/ru/guides/formulas.mdx` | Create |

---

### Task 1: Update astro.config.mjs — i18n + full sidebar

**Files:**
- Modify: `astro.config.mjs`

- [ ] **Step 1: Replace `astro.config.mjs` with the full config**

```js
// @ts-check
import { defineConfig } from 'astro/config';
import starlight from '@astrojs/starlight';

export default defineConfig({
  integrations: [
    starlight({
      title: 'Math On Canvas',
      logo: {
        src: './src/assets/logo.svg',
        alt: 'Math On Canvas',
      },
      customCss: ['./src/styles/theme.css'],
      components: {
        ThemeSelect: './src/components/starlight/ThemeSelect.astro',
      },
      locales: {
        root: { label: 'English', lang: 'en' },
        ru:   { label: 'Русский', lang: 'ru' },
        de:   { label: 'Deutsch', lang: 'de' },
        es:   { label: 'Español', lang: 'es' },
      },
      defaultLocale: 'root',
      sidebar: [
        {
          label: 'Getting Started',
          translations: { ru: 'Начало работы', de: 'Erste Schritte', es: 'Empezando' },
          items: [
            { label: 'Introduction', slug: 'getting-started/introduction',
              translations: { ru: 'Введение', de: 'Einführung', es: 'Introducción' } },
            { label: 'Quick Start', slug: 'getting-started/quick-start',
              translations: { ru: 'Быстрый старт', de: 'Schnellstart', es: 'Inicio rápido' } },
            { label: 'Interface Overview', slug: 'getting-started/interface',
              translations: { ru: 'Обзор интерфейса', de: 'Oberfläche', es: 'Interfaz' } },
          ],
        },
        {
          label: 'Guides',
          translations: { ru: 'Руководства', de: 'Anleitungen', es: 'Guías' },
          items: [
            { label: 'Formulas', slug: 'guides/formulas',
              translations: { ru: 'Формулы', de: 'Formeln', es: 'Fórmulas' } },
            { label: 'Mathematical Symbols', slug: 'guides/symbols',
              translations: { ru: 'Математические символы', de: 'Mathematische Symbole', es: 'Símbolos matemáticos' } },
            {
              label: '2D Shapes',
              translations: { ru: '2D-фигуры', de: '2D-Formen', es: 'Figuras 2D' },
              items: [
                { label: 'Overview', slug: 'guides/shapes-2d',
                  translations: { ru: 'Обзор', de: 'Übersicht', es: 'Resumen' } },
                { label: 'Circle', slug: 'guides/shapes-2d/circle',
                  translations: { ru: 'Окружность', de: 'Kreis', es: 'Círculo' } },
                { label: 'Rectangle', slug: 'guides/shapes-2d/rectangle',
                  translations: { ru: 'Прямоугольник', de: 'Rechteck', es: 'Rectángulo' } },
                { label: 'Triangle', slug: 'guides/shapes-2d/triangle',
                  translations: { ru: 'Треугольник', de: 'Dreieck', es: 'Triángulo' } },
                { label: 'Polygon & Polyline', slug: 'guides/shapes-2d/polygon',
                  translations: { ru: 'Многоугольник и ломаная', de: 'Polygon', es: 'Polígono' } },
                { label: 'Line & Arrow', slug: 'guides/shapes-2d/line-arrow',
                  translations: { ru: 'Линия и стрелка', de: 'Linie & Pfeil', es: 'Línea y flecha' } },
              ],
            },
            {
              label: '3D Shapes',
              translations: { ru: '3D-фигуры', de: '3D-Formen', es: 'Figuras 3D' },
              items: [
                { label: 'Overview', slug: 'guides/shapes-3d',
                  translations: { ru: 'Обзор', de: 'Übersicht', es: 'Resumen' } },
                { label: 'Cube', slug: 'guides/shapes-3d/cube',
                  translations: { ru: 'Куб', de: 'Würfel', es: 'Cubo' } },
                { label: 'Sphere', slug: 'guides/shapes-3d/sphere',
                  translations: { ru: 'Сфера', de: 'Kugel', es: 'Esfera' } },
                { label: 'Cone', slug: 'guides/shapes-3d/cone',
                  translations: { ru: 'Конус', de: 'Kegel', es: 'Cono' } },
                { label: 'Cylinder', slug: 'guides/shapes-3d/cylinder',
                  translations: { ru: 'Цилиндр', de: 'Zylinder', es: 'Cilindro' } },
                { label: 'Prism', slug: 'guides/shapes-3d/prism',
                  translations: { ru: 'Призма', de: 'Prisma', es: 'Prisma' } },
                { label: 'Pyramid', slug: 'guides/shapes-3d/pyramid',
                  translations: { ru: 'Пирамида', de: 'Pyramide', es: 'Pirámide' } },
              ],
            },
            { label: 'Text', slug: 'guides/text',
              translations: { ru: 'Текст', de: 'Text', es: 'Texto' } },
            { label: 'Drawing', slug: 'guides/drawing',
              translations: { ru: 'Рисование', de: 'Zeichnen', es: 'Dibujo' } },
            { label: 'Images', slug: 'guides/images',
              translations: { ru: 'Изображения', de: 'Bilder', es: 'Imágenes' } },
            { label: 'Annotations', slug: 'guides/annotations',
              translations: { ru: 'Аннотации', de: 'Anmerkungen', es: 'Anotaciones' } },
            { label: 'Connections & Vertices', slug: 'guides/connections',
              translations: { ru: 'Связи и вершины', de: 'Verbindungen', es: 'Conexiones' } },
            { label: 'Pages & Documents', slug: 'guides/pages-documents',
              translations: { ru: 'Страницы и документы', de: 'Seiten & Dokumente', es: 'Páginas y documentos' } },
            { label: 'AI Assistant', slug: 'guides/ai-assistant',
              translations: { ru: 'ИИ-ассистент', de: 'KI-Assistent', es: 'Asistente IA' } },
            { label: 'Export', slug: 'guides/export',
              translations: { ru: 'Экспорт', de: 'Exportieren', es: 'Exportar' } },
          ],
        },
        {
          label: 'Reference',
          translations: { ru: 'Справочник', de: 'Referenz', es: 'Referencia' },
          items: [
            { label: 'Keyboard Shortcuts', slug: 'reference/keyboard-shortcuts',
              translations: { ru: 'Горячие клавиши', de: 'Tastaturkürzel', es: 'Atajos de teclado' } },
            { label: 'Element Properties', slug: 'reference/element-properties',
              translations: { ru: 'Свойства элементов', de: 'Elementeigenschaften', es: 'Propiedades de elementos' } },
            { label: 'Supported Formats', slug: 'reference/export-formats',
              translations: { ru: 'Форматы экспорта', de: 'Exportformate', es: 'Formatos de exportación' } },
          ],
        },
      ],
    }),
  ],
});
```

- [ ] **Step 2: Delete old placeholder files**

```bash
cd C:\Work\Projects\React\math-on-canvas-doc
rm src/content/docs/guides/example.md
rm src/content/docs/reference/example.md
```

- [ ] **Step 3: Verify build doesn't break yet (expected: broken links — that's OK)**

```bash
npm run build 2>&1 | tail -20
```

Expected: build may warn about missing slugs referenced in sidebar. This is expected — content files are created in subsequent tasks.

- [ ] **Step 4: Commit**

```bash
git add astro.config.mjs
git rm src/content/docs/guides/example.md src/content/docs/reference/example.md
git commit -m "configure i18n, full sidebar, remove placeholder files"
```

---

### Task 2: Homepage and Getting Started (EN)

**Files:**
- Rewrite: `src/content/docs/index.mdx`
- Create: `src/content/docs/getting-started/introduction.md`
- Create: `src/content/docs/getting-started/quick-start.mdx`
- Create: `src/content/docs/getting-started/interface.md`

- [ ] **Step 1: Rewrite homepage `src/content/docs/index.mdx`**

```mdx
---
title: Math Poster Editor Documentation
description: Official documentation for Math On Canvas — a browser-based editor for mathematical posters and diagrams. LaTeX formulas, 2D/3D shapes, AI assistant, and PDF/SVG/PNG export.
template: splash
hero:
  tagline: Create beautiful mathematical posters and diagrams in your browser.
  actions:
    - text: Get Started
      link: /getting-started/introduction/
      icon: right-arrow
    - text: Quick Start
      link: /getting-started/quick-start/
      icon: rocket
      variant: minimal
---
```

- [ ] **Step 2: Create `src/content/docs/getting-started/introduction.md`**

```md
---
title: Math Poster Editor — Math On Canvas
description: Create mathematical posters and diagrams in your browser. Add LaTeX formulas, 2D and 3D shapes, text, and images. Export to PDF, SVG, or PNG.
---

# Math Poster Editor

Math On Canvas is a browser-based editor for creating mathematical posters and diagrams — no installation required.

## What You Can Create

## Element Types

## Guest vs Authenticated Features

| Feature | Guest | Authenticated |
|---|---|---|
| Full canvas editing | ✓ | ✓ |
| All element types | ✓ | ✓ |
| PDF / SVG / PNG export | ✓ | ✓ |
| AI formula generation | ✗ | ✓ |
| AI document generation | ✗ | ✓ |
| Cloud import / export | ✗ | ✓ |

## Next Steps
```

- [ ] **Step 3: Create `src/content/docs/getting-started/quick-start.mdx`**

Before writing this file, get the YouTube Quick Start video ID from the project owner and substitute `YOUTUBE_QUICK_START_ID` below.

```mdx
---
title: Quick Start Guide — Math On Canvas
description: Create your first math poster in 5 minutes. Step-by-step guide to adding formulas, shapes, and exporting your diagram.
---

# Quick Start Guide

Create your first mathematical poster in 5 minutes.

<iframe
  width="100%"
  style="aspect-ratio: 16/9; border: none; border-radius: 8px;"
  src="https://www.youtube.com/embed/YOUTUBE_QUICK_START_ID"
  allowfullscreen
/>

## Step 1: Open the Editor

## Step 2: Add Your First Formula

## Step 3: Add Shapes

## Step 4: Arrange Elements

## Step 5: Export Your Poster

## Related

- [Interface Overview](/getting-started/interface/)
- [Formulas](/guides/formulas/)
- [Export](/guides/export/)
```

- [ ] **Step 4: Create `src/content/docs/getting-started/interface.md`**

```md
---
title: Interface Overview — Math On Canvas
description: Learn the Math On Canvas editor interface — Toolbar, EditorBar, Canvas, Sidebar panels, and floating Palettes.
---

# Interface Overview

Math On Canvas has five main interface areas that work together to let you create and edit diagrams.

## Toolbar

The top bar with page controls, canvas tools, and document actions.

### Page Controls (left)

### Canvas Tools (center)

### Document Actions (right)

## EditorBar

The vertical bar on the left of the canvas with quick-insert buttons for all element types.

## Canvas

The main editing area powered by Konva.js. Supports pan, zoom, and multi-element selection.

## Sidebar

The right panel with three sections: Pages, Elements, and Properties.

### Pages Panel

### Elements Panel

### Properties Panel

## Palettes

Draggable floating panels for Math Symbols, Predefined Formulas, Shape selection, 3D Shape selection, and Free Draw settings.

## Related

- [Quick Start](/getting-started/quick-start/)
- [Formulas](/guides/formulas/)
- [2D Shapes](/guides/shapes-2d/index/)
```

- [ ] **Step 5: Commit**

```bash
git add src/content/docs/index.mdx src/content/docs/getting-started/
git commit -m "scaffold homepage and getting started section (EN)"
```

---

### Task 3: Guides — Flat Pages (EN)

**Files:**
- Create: `src/content/docs/guides/formulas.mdx`
- Create: `src/content/docs/guides/symbols.md`
- Create: `src/content/docs/guides/text.md`
- Create: `src/content/docs/guides/drawing.md`
- Create: `src/content/docs/guides/images.md`
- Create: `src/content/docs/guides/annotations.md`
- Create: `src/content/docs/guides/connections.md`
- Create: `src/content/docs/guides/pages-documents.md`
- Create: `src/content/docs/guides/ai-assistant.md`
- Create: `src/content/docs/guides/export.md`

- [ ] **Step 1: Create `src/content/docs/guides/formulas.mdx`**

Get the YouTube formula/LaTeX video ID from the project owner and substitute `YOUTUBE_FORMULAS_ID` if a video exists for this page; otherwise remove the iframe block.

```mdx
---
title: LaTeX Formula Editor — Math On Canvas
description: Add and edit math formulas using LaTeX syntax. Live preview with MathJax, 150+ symbols, and 60+ predefined formulas for algebra and geometry.
---

# LaTeX Formula Editor

Add mathematical formulas to your poster using LaTeX syntax with live MathJax preview.

<iframe
  width="100%"
  style="aspect-ratio: 16/9; border: none; border-radius: 8px;"
  src="https://www.youtube.com/embed/YOUTUBE_FORMULAS_ID"
  allowfullscreen
/>

## Adding a Formula

## The LaTeX Editor

## Math Symbol Palette

## Predefined Formulas

## Formula Properties

## Related

- [Mathematical Symbols](/guides/symbols/)
- [Quick Start](/getting-started/quick-start/)
```

- [ ] **Step 2: Create `src/content/docs/guides/symbols.md`**

```md
---
title: Math Symbol Palette — Math On Canvas
description: Browse 150+ mathematical symbols organized by category — Greek letters, set theory, calculus, logic, geometry, and more. Click to insert as a formula.
---

# Math Symbol Palette

The Math Symbol Palette gives quick access to 150+ mathematical symbols organized by category.

## Opening the Palette

## Symbol Categories

### Greek Letters

### Set Theory

### Relations

### Geometry

### Arithmetic Operators

### Calculus

### Logic

### Arrows

### Constants

## Inserting a Symbol

## Related

- [Formulas](/guides/formulas/)
```

- [ ] **Step 3: Create `src/content/docs/guides/text.md`**

```md
---
title: Adding Text — Math On Canvas
description: Add text boxes to your math diagram. Choose font family, size, color, bold or italic style, and optional fixed width.
---

# Adding Text

Add plain text boxes to label or annotate your math poster.

## Adding a Text Element

## Text Properties

### Font Family

### Font Size

### Color

### Bold and Italic

### Fixed Width

## Editing Text

## Related

- [Formulas](/guides/formulas/)
- [Annotations](/guides/annotations/)
```

- [ ] **Step 4: Create `src/content/docs/guides/drawing.md`**

```md
---
title: Freehand Drawing — Math On Canvas
description: Draw freely on the canvas using Pencil or Marker tools. Customize stroke color, width, opacity, and line style.
---

# Freehand Drawing

Use freehand drawing tools to sketch directly on the canvas.

## Pencil Tool

## Marker Tool

## Free Draw Properties

### Stroke Color

### Stroke Width

### Opacity

### Line Cap and Join

## Related

- [Images](/guides/images/)
```

- [ ] **Step 5: Create `src/content/docs/guides/images.md`**

```md
---
title: Adding Images — Math On Canvas
description: Upload raster images to your math diagram. Resize with optional aspect-ratio lock and position freely on the canvas.
---

# Adding Images

Insert raster images from your local files into the poster.

## Uploading an Image

## Resizing an Image

## Aspect-Ratio Lock

## Image Properties

## Related

- [Drawing](/guides/drawing/)
- [Export](/guides/export/)
```

- [ ] **Step 6: Create `src/content/docs/guides/annotations.md`**

```md
---
title: Angle Annotations — Math On Canvas
description: Add angle annotations with arc, square, or dot style. Display angle values with customizable font size and side annotations.
---

# Angle Annotations

Annotate angles in your diagrams with visual markers and numeric labels.

## Adding an Annotation

## Annotation Styles

### Arc

### Square (Right Angle)

### Dot

## Angle Value Display

## Side Annotations

## Related

- [2D Shapes](/guides/shapes-2d/index/)
- [Connections & Vertices](/guides/connections/)
```

- [ ] **Step 7: Create `src/content/docs/guides/connections.md`**

```md
---
title: Connections and Vertices — Math On Canvas
description: Connect shapes with snap-to-vertex. Enable vertex mode, set snap tolerance, and move connected elements together automatically.
---

# Connections and Vertices

Link shapes together so they stay connected when you move them.

## Enabling Vertex Mode

## Snap Tolerance

## Connecting Two Shapes

## Moving Connected Elements

## Disconnecting Elements

## Related

- [2D Shapes](/guides/shapes-2d/index/)
- [Line & Arrow](/guides/shapes-2d/line-arrow/)
```

- [ ] **Step 8: Create `src/content/docs/guides/pages-documents.md`**

```md
---
title: Pages and Documents — Math On Canvas
description: Manage multi-page documents. Add, delete, duplicate, and rename pages. Choose A4, Letter, or Poster format in portrait or landscape.
---

# Pages and Documents

Math On Canvas supports multi-page documents for creating poster series or step-by-step diagram sequences.

## Page Formats

| Format | Dimensions |
|---|---|
| A4 | 210 × 297 mm |
| Letter | 216 × 279 mm |
| Poster | 432 × 558 mm (2× Letter) |

## Portrait and Landscape

## Managing Pages

### Adding a Page

### Duplicating a Page

### Renaming a Page

### Deleting a Page

## New Document

## Cloud Import / Export

## Related

- [Export](/guides/export/)
```

- [ ] **Step 9: Create `src/content/docs/guides/ai-assistant.md`**

```md
---
title: AI Assistant — Math On Canvas
description: Generate math formulas or entire posters from a text prompt. AI Formula and Document tabs with real-time job status. Requires a free account.
---

# AI Assistant

The AI Assistant generates LaTeX formulas or complete diagrams from a natural-language description.

:::note
The AI Assistant requires a free Math On Canvas account. [Sign up](#) to get started.
:::

## Opening the AI Panel

## Formula Tab

## Document Tab

## Job Status

## Cancelling a Job

## Error States

## Related

- [Formulas](/guides/formulas/)
- [Pages & Documents](/guides/pages-documents/)
```

- [ ] **Step 10: Create `src/content/docs/guides/export.md`**

```md
---
title: Export to PDF, SVG, PNG — Math On Canvas
description: Export your math poster as PDF, SVG, or PNG. Select individual pages or export all. Multi-page exports are packaged as a ZIP file.
---

# Export to PDF, SVG, PNG

Export your finished poster in three formats: PDF for print, SVG for vector graphics, or PNG for raster images.

## Opening the Export Dialog

## Selecting Pages

## PDF Export

## SVG Export

## PNG Export

## Multi-Page Export (ZIP)

## Cloud Export

## Related

- [Pages & Documents](/guides/pages-documents/)
- [Supported Formats](/reference/export-formats/)
```

- [ ] **Step 11: Commit**

```bash
git add src/content/docs/guides/
git commit -m "scaffold flat guide pages (EN)"
```

---

### Task 4: Guides — 2D Shapes Sub-section (EN)

**Files:**
- Create: `src/content/docs/guides/shapes-2d/index.md`
- Create: `src/content/docs/guides/shapes-2d/circle.md`
- Create: `src/content/docs/guides/shapes-2d/rectangle.md`
- Create: `src/content/docs/guides/shapes-2d/triangle.md`
- Create: `src/content/docs/guides/shapes-2d/polygon.md`
- Create: `src/content/docs/guides/shapes-2d/line-arrow.md`

- [ ] **Step 1: Create `src/content/docs/guides/shapes-2d/index.md`**

Get the YouTube 2D shapes video ID if one exists and add an iframe embed. Otherwise omit it.

```md
---
title: 2D Shapes — Math On Canvas
description: Draw 14 types of 2D geometric shapes — circles, polygons, arrows, arcs, and more. Customize stroke color, fill color, opacity, and dash pattern.
---

# 2D Shapes

Math On Canvas includes 14 types of 2D shapes for building geometric diagrams and math illustrations.

## Available Shapes

| Shape | Description |
|---|---|
| Circle | Perfect circle with optional center marker |
| Oval | Ellipse |
| Rectangle | Rectangular box |
| Triangle | Equilateral triangle |
| Pentagon | Regular 5-sided polygon |
| Hexagon | Regular 6-sided polygon |
| Polygon | Arbitrary vertex list |
| Polyline | Open arbitrary vertex list |
| Parallelogram | |
| Rhombus | |
| Trapezoid | |
| Arc | Circular arc |
| Sector | Circular wedge |
| Arrow Line | Line with configurable endpoints |
| Page Frame | Page boundary rectangle |
| Page Table | Grid with configurable rows and columns |

## Common Shape Properties

### Stroke Color

### Fill Color and Opacity

### Stroke Width

### Dash Pattern

## Inserting a Shape

## Related

- [Connections & Vertices](/guides/connections/)
- [3D Shapes](/guides/shapes-3d/index/)
```

- [ ] **Step 2: Create `src/content/docs/guides/shapes-2d/circle.md`**

```md
---
title: Drawing Circles — Math On Canvas
description: Add circles to your math diagram. Set stroke color, fill color, opacity, dash pattern, and add an optional center point marker.
---

# Drawing Circles

Add a perfect circle to your diagram from the Shape Palette.

## Inserting a Circle

## Circle Properties

### Stroke Color

### Fill Color

### Fill Opacity

### Stroke Width

### Dash Pattern

### Center Marker

## Related

- [2D Shapes Overview](/guides/shapes-2d/)
- [Rectangle](/guides/shapes-2d/rectangle/)
- [Connections & Vertices](/guides/connections/)
```

- [ ] **Step 3: Create `src/content/docs/guides/shapes-2d/rectangle.md`**

```md
---
title: Drawing Rectangles — Math On Canvas
description: Add rectangles to your math diagram. Customize stroke color, fill color, fill opacity, stroke width, and dash pattern.
---

# Drawing Rectangles

Add a rectangle to your diagram from the Shape Palette.

## Inserting a Rectangle

## Rectangle Properties

### Stroke Color

### Fill Color

### Fill Opacity

### Stroke Width

### Dash Pattern

## Related

- [2D Shapes Overview](/guides/shapes-2d/)
- [Connections & Vertices](/guides/connections/)
```

- [ ] **Step 4: Create `src/content/docs/guides/shapes-2d/triangle.md`**

```md
---
title: Drawing Triangles — Math On Canvas
description: Add equilateral triangles to your math poster. Set stroke color, fill color, opacity, stroke width, and dash pattern.
---

# Drawing Triangles

Add an equilateral triangle to your diagram from the Shape Palette.

## Inserting a Triangle

## Triangle Properties

### Stroke Color

### Fill Color

### Fill Opacity

### Stroke Width

### Dash Pattern

## Related

- [2D Shapes Overview](/guides/shapes-2d/)
- [Polygon & Polyline](/guides/shapes-2d/polygon/)
```

- [ ] **Step 5: Create `src/content/docs/guides/shapes-2d/polygon.md`**

```md
---
title: Polygons and Polylines — Math On Canvas
description: Draw custom polygons and polylines with an arbitrary vertex list. Set stroke color, fill color, opacity, stroke width, and dash pattern.
---

# Polygons and Polylines

Draw custom closed polygons or open polylines by placing vertices on the canvas.

## Drawing a Polygon

## Drawing a Polyline

## Editing Vertices

## Polygon Properties

### Stroke Color

### Fill Color

### Fill Opacity

### Stroke Width

### Dash Pattern

## Related

- [2D Shapes Overview](/guides/shapes-2d/)
- [Line & Arrow](/guides/shapes-2d/line-arrow/)
- [Connections & Vertices](/guides/connections/)
```

- [ ] **Step 6: Create `src/content/docs/guides/shapes-2d/line-arrow.md`**

```md
---
title: Lines and Arrows — Math On Canvas
description: Add arrow lines with configurable endpoint styles — arrow, triangle, circle, square, or bar. Set stroke color, width, and dash pattern.
---

# Lines and Arrows

Add directed lines with customizable endpoint markers to show relationships between elements.

## Inserting a Line or Arrow

## Endpoint Styles

| Style | Description |
|---|---|
| None | Plain line end |
| Arrow | Standard arrowhead |
| Triangle | Filled triangle |
| Circle | Filled circle |
| Square | Filled square |
| Bar | Perpendicular bar |

## Line Properties

### Stroke Color

### Stroke Width

### Dash Pattern

## Connecting to Shapes

## Related

- [2D Shapes Overview](/guides/shapes-2d/)
- [Connections & Vertices](/guides/connections/)
```

- [ ] **Step 7: Commit**

```bash
git add src/content/docs/guides/shapes-2d/
git commit -m "scaffold 2D shapes guide pages (EN)"
```

---

### Task 5: Guides — 3D Shapes Sub-section (EN)

**Files:**
- Create: `src/content/docs/guides/shapes-3d/index.md`
- Create: `src/content/docs/guides/shapes-3d/cube.md`
- Create: `src/content/docs/guides/shapes-3d/sphere.md`
- Create: `src/content/docs/guides/shapes-3d/cone.md`
- Create: `src/content/docs/guides/shapes-3d/cylinder.md`
- Create: `src/content/docs/guides/shapes-3d/prism.md`
- Create: `src/content/docs/guides/shapes-3d/pyramid.md`

- [ ] **Step 1: Create `src/content/docs/guides/shapes-3d/index.md`**

Get the YouTube 3D shapes video ID if one exists and add an iframe embed. Otherwise omit it.

```md
---
title: 3D Shapes — Math On Canvas
description: Draw 7 types of 3D geometric shapes with software projection. Set dimensions, rotation, face opacity, and optional vertex labels.
---

# 3D Shapes

Math On Canvas renders 3D shapes using a software projection engine — no WebGL required.

## Available 3D Shapes

| Shape | Description |
|---|---|
| Cube | Rectangular box |
| Prism | Triangular prism |
| Pyramid (3-sided) | Triangular pyramid |
| Pyramid (general) | Configurable base |
| Sphere | |
| Cone | |
| Cylinder | |

## Common 3D Shape Properties

### Dimensions (Width / Height / Depth)

### Rotation Matrix

### Face Opacity

### Vertex Labels

### Center Point Marker

## Inserting a 3D Shape

## Related

- [2D Shapes](/guides/shapes-2d/index/)
```

- [ ] **Step 2: Create `src/content/docs/guides/shapes-3d/cube.md`**

```md
---
title: Drawing Cubes — Math On Canvas
description: Add a cube to your math diagram. Set width, height, depth, 3×3 rotation matrix, face opacity, and optional vertex labels.
---

# Drawing Cubes

Add a cube (rectangular box) to your math diagram.

## Inserting a Cube

## Cube Properties

### Width, Height, Depth

### Rotation

### Face Opacity

### Vertex Labels

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Prism](/guides/shapes-3d/prism/)
```

- [ ] **Step 3: Create `src/content/docs/guides/shapes-3d/sphere.md`**

```md
---
title: Drawing Spheres — Math On Canvas
description: Add a sphere to your math diagram. Set dimensions, rotation, face opacity, and an optional center point marker.
---

# Drawing Spheres

Add a sphere to your math diagram with configurable opacity and rotation.

## Inserting a Sphere

## Sphere Properties

### Dimensions

### Rotation

### Face Opacity

### Center Point Marker

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Cone](/guides/shapes-3d/cone/)
```

- [ ] **Step 4: Create `src/content/docs/guides/shapes-3d/cone.md`**

```md
---
title: Drawing Cones — Math On Canvas
description: Add a cone to your math diagram. Set base radius, height, rotation matrix, and face opacity.
---

# Drawing Cones

Add a cone to your math diagram.

## Inserting a Cone

## Cone Properties

### Dimensions

### Rotation

### Face Opacity

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Cylinder](/guides/shapes-3d/cylinder/)
- [Pyramid](/guides/shapes-3d/pyramid/)
```

- [ ] **Step 5: Create `src/content/docs/guides/shapes-3d/cylinder.md`**

```md
---
title: Drawing Cylinders — Math On Canvas
description: Add a cylinder to your math diagram. Set radius, height, rotation matrix, and face opacity.
---

# Drawing Cylinders

Add a cylinder to your math diagram.

## Inserting a Cylinder

## Cylinder Properties

### Dimensions

### Rotation

### Face Opacity

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Cone](/guides/shapes-3d/cone/)
- [Prism](/guides/shapes-3d/prism/)
```

- [ ] **Step 6: Create `src/content/docs/guides/shapes-3d/prism.md`**

```md
---
title: Drawing Prisms — Math On Canvas
description: Add a triangular prism to your math diagram. Set dimensions, rotation matrix, face opacity, and optional vertex labels.
---

# Drawing Prisms

Add a triangular prism to your math diagram.

## Inserting a Prism

## Prism Properties

### Dimensions

### Rotation

### Face Opacity

### Vertex Labels

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Cube](/guides/shapes-3d/cube/)
- [Pyramid](/guides/shapes-3d/pyramid/)
```

- [ ] **Step 7: Create `src/content/docs/guides/shapes-3d/pyramid.md`**

```md
---
title: Drawing Pyramids — Math On Canvas
description: Add a 3-sided or general pyramid to your math diagram. Set base dimensions, height, rotation, face opacity, and optional vertex labels.
---

# Drawing Pyramids

Add a pyramid to your math diagram. Math On Canvas supports 3-sided pyramids and general pyramids with configurable base polygons.

## Inserting a Pyramid

## Pyramid Types

### 3-Sided Pyramid (Tetrahedron)

### General Pyramid

## Pyramid Properties

### Dimensions

### Rotation

### Face Opacity

### Vertex Labels

## Related

- [3D Shapes Overview](/guides/shapes-3d/)
- [Cone](/guides/shapes-3d/cone/)
- [Prism](/guides/shapes-3d/prism/)
```

- [ ] **Step 8: Commit**

```bash
git add src/content/docs/guides/shapes-3d/
git commit -m "scaffold 3D shapes guide pages (EN)"
```

---

### Task 6: Reference Section (EN)

**Files:**
- Create: `src/content/docs/reference/keyboard-shortcuts.md`
- Create: `src/content/docs/reference/element-properties.md`
- Create: `src/content/docs/reference/export-formats.md`

- [ ] **Step 1: Create `src/content/docs/reference/keyboard-shortcuts.md`**

```md
---
title: Keyboard Shortcuts — Math On Canvas
description: Complete keyboard shortcut reference for Math On Canvas — arrow-key nudge, Delete, Undo, Redo, Escape, and zoom controls.
---

# Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Undo | Ctrl+Z |
| Redo | Ctrl+Y / Ctrl+Shift+Z |
| Delete selected | Delete / Backspace |
| Nudge element | Arrow keys |
| Cancel / close | Escape |
| Zoom in | Ctrl+= |
| Zoom out | Ctrl+- |
| Reset zoom | Ctrl+0 |

## Canvas Navigation

## Element Selection

## Text and Formula Editing

## Related

- [Interface Overview](/getting-started/interface/)
```

- [ ] **Step 2: Create `src/content/docs/reference/element-properties.md`**

```md
---
title: Element Properties Reference — Math On Canvas
description: Full reference of all properties for every element type — position, rotation, colors, fonts, dimensions, and more.
---

# Element Properties Reference

All elements share common position and rotation properties. Additional properties depend on the element type.

## Common Properties

| Property | Description |
|---|---|
| x, y | Position on canvas |
| rotation | Rotation angle in degrees |
| id | Unique element identifier |

## Text

| Property | Values |
|---|---|
| fontFamily | Font name |
| fontSize | Size in px |
| color | Hex color |
| bold | true / false |
| italic | true / false |
| fixedWidth | Width in px or null |

## Formula

| Property | Values |
|---|---|
| latex | LaTeX string |
| fontSize | Size in px |
| fontFamily | Font name |
| color | Hex color |
| inline | true / false |

## 2D Shape (all kinds)

| Property | Values |
|---|---|
| strokeColor | Hex color |
| fillColor | Hex color |
| fillOpacity | 0–1 |
| strokeWidth | px |
| dash | solid / dashed / dotted / dash-dot |

## 3D Shape (all kinds)

| Property | Values |
|---|---|
| width / height / depth | px |
| rotationMatrix | 3×3 matrix |
| faceOpacity | 0–1 |
| vertexLabels | true / false |
| centerMarker | true / false |

## Image

| Property | Values |
|---|---|
| src | Data URL |
| width / height | px |
| aspectRatioLocked | true / false |

## Freehand Draw

| Property | Values |
|---|---|
| strokeColor | Hex color |
| strokeWidth | px |
| opacity | 0–1 |
| tool | pencil / marker |
```

- [ ] **Step 3: Create `src/content/docs/reference/export-formats.md`**

```md
---
title: Supported Export Formats — Math On Canvas
description: Technical details of PDF, SVG, and PNG export — font embedding, multi-page ZIP packaging, and format specifications.
---

# Supported Export Formats

## PDF

## SVG

## PNG

## Multi-Page Export

When exporting a document with multiple pages, Math On Canvas packages the files into a single ZIP archive.

## Font Embedding

PDF and SVG exports embed the NotoSansMath and Roboto fonts to ensure formulas render correctly on any device.

## Related

- [Export Guide](/guides/export/)
- [Pages & Documents](/guides/pages-documents/)
```

- [ ] **Step 4: Commit**

```bash
git add src/content/docs/reference/
git commit -m "scaffold reference section (EN)"
```

---

### Task 7: RU Priority Pages

**Files:**
- Create: `src/content/docs/ru/index.mdx`
- Create: `src/content/docs/ru/getting-started/introduction.md`
- Create: `src/content/docs/ru/getting-started/quick-start.mdx`
- Create: `src/content/docs/ru/getting-started/interface.md`
- Create: `src/content/docs/ru/guides/formulas.mdx`

- [ ] **Step 1: Create `src/content/docs/ru/index.mdx`**

```mdx
---
title: Документация Math On Canvas
description: Официальная документация Math On Canvas — браузерный редактор математических плакатов и диаграмм. LaTeX-формулы, 2D/3D-фигуры, ИИ-ассистент, экспорт в PDF/SVG/PNG.
template: splash
hero:
  tagline: Создавайте красивые математические плакаты и диаграммы прямо в браузере.
  actions:
    - text: Начать работу
      link: /ru/getting-started/introduction/
      icon: right-arrow
    - text: Быстрый старт
      link: /ru/getting-started/quick-start/
      icon: rocket
      variant: minimal
---
```

- [ ] **Step 2: Create `src/content/docs/ru/getting-started/introduction.md`**

```md
---
title: Редактор математических плакатов — Math On Canvas
description: Создавайте математические плакаты и диаграммы в браузере. Добавляйте LaTeX-формулы, 2D- и 3D-фигуры, текст и изображения. Экспорт в PDF, SVG и PNG.
---

# Редактор математических плакатов

Math On Canvas — браузерный редактор для создания математических плакатов и диаграмм. Установка не требуется.

## Что можно создавать

## Типы элементов

## Возможности: гость и авторизованный пользователь

| Функция | Гость | Авторизован |
|---|---|---|
| Полное редактирование | ✓ | ✓ |
| Все типы элементов | ✓ | ✓ |
| Экспорт PDF / SVG / PNG | ✓ | ✓ |
| Генерация формул ИИ | ✗ | ✓ |
| Генерация документа ИИ | ✗ | ✓ |
| Облачный импорт / экспорт | ✗ | ✓ |

## Следующие шаги
```

- [ ] **Step 3: Create `src/content/docs/ru/getting-started/quick-start.mdx`**

Use the same YouTube video ID as the EN version (`YOUTUBE_QUICK_START_ID`).

```mdx
---
title: Быстрый старт — Math On Canvas
description: Создайте первый математический плакат за 5 минут. Пошаговое руководство по добавлению формул, фигур и экспорту диаграммы.
---

# Быстрый старт

Создайте первый математический плакат за 5 минут.

<iframe
  width="100%"
  style="aspect-ratio: 16/9; border: none; border-radius: 8px;"
  src="https://www.youtube.com/embed/YOUTUBE_QUICK_START_ID"
  allowfullscreen
/>

> Видео на английском языке. Субтитры на русском доступны — нажмите CC в плеере.

## Шаг 1: Открыть редактор

## Шаг 2: Добавить первую формулу

## Шаг 3: Добавить фигуры

## Шаг 4: Расставить элементы

## Шаг 5: Экспортировать плакат

## Смотрите также

- [Обзор интерфейса](/ru/getting-started/interface/)
- [Формулы](/ru/guides/formulas/)
- [Экспорт](/ru/guides/export/)
```

- [ ] **Step 4: Create `src/content/docs/ru/getting-started/interface.md`**

```md
---
title: Обзор интерфейса — Math On Canvas
description: Изучите интерфейс редактора Math On Canvas — панель инструментов, боковая панель, холст, панели свойств и плавающие палитры.
---

# Обзор интерфейса

Редактор Math On Canvas состоит из пяти основных областей.

## Панель инструментов (Toolbar)

Верхняя панель с управлением страницами, инструментами холста и действиями с документом.

### Управление страницами (слева)

### Инструменты холста (центр)

### Действия с документом (справа)

## Панель вставки (EditorBar)

Вертикальная панель слева от холста с кнопками быстрой вставки всех типов элементов.

## Холст (Canvas)

Основная рабочая область на базе Konva.js. Поддерживает прокрутку, масштабирование и выбор нескольких элементов.

## Боковая панель (Sidebar)

Правая панель с тремя разделами: Страницы, Элементы и Свойства.

### Страницы

### Элементы

### Свойства

## Палитры (Palettes)

Перемещаемые панели: математические символы, готовые формулы, 2D-фигуры, 3D-фигуры, параметры рисования.

## Смотрите также

- [Быстрый старт](/ru/getting-started/quick-start/)
- [Формулы](/ru/guides/formulas/)
```

- [ ] **Step 5: Create `src/content/docs/ru/guides/formulas.mdx`**

Use the same YouTube formula video ID as the EN version, or omit the iframe if no formula video exists.

```mdx
---
title: Редактор LaTeX-формул — Math On Canvas
description: Добавляйте и редактируйте математические формулы с помощью LaTeX. Живой предпросмотр MathJax, 150+ символов и 60+ готовых формул по алгебре и геометрии.
---

# Редактор LaTeX-формул

Добавляйте математические формулы в плакат, используя синтаксис LaTeX с живым предпросмотром через MathJax.

<iframe
  width="100%"
  style="aspect-ratio: 16/9; border: none; border-radius: 8px;"
  src="https://www.youtube.com/embed/YOUTUBE_FORMULAS_ID"
  allowfullscreen
/>

> Видео на английском языке. Субтитры на русском доступны — нажмите CC в плеере.

## Добавление формулы

## Редактор LaTeX

## Палитра математических символов

## Готовые формулы

## Свойства формулы

## Смотрите также

- [Математические символы](/ru/guides/symbols/)
- [Быстрый старт](/ru/getting-started/quick-start/)
```

- [ ] **Step 6: Commit**

```bash
git add src/content/docs/ru/
git commit -m "scaffold RU priority pages (homepage, getting started, formulas)"
```

---

### Task 8: Build Verification

**Files:** none changed

- [ ] **Step 1: Run the build**

```bash
cd C:\Work\Projects\React\math-on-canvas-doc
npm run build 2>&1
```

Expected: `4+ page(s) built` with no errors. Warnings about `YOUTUBE_*_ID` placeholders in iframes are acceptable — those are real URLs that need real IDs before publishing.

- [ ] **Step 2: Start dev server and verify sidebar**

```bash
npm run dev
```

Open `http://localhost:4321` and check:
- [ ] Sidebar shows all three top-level groups: Getting Started, Guides, Reference
- [ ] Guides group has 2D Shapes and 3D Shapes as collapsible sub-groups
- [ ] All sidebar links navigate to pages without 404
- [ ] Language switcher shows EN / RU / DE / ES options
- [ ] Switching to RU shows Russian sidebar labels and RU content for priority pages
- [ ] Switching to DE falls back to EN content (expected — no DE content yet)
- [ ] Logo appears in top-left nav

- [ ] **Step 3: Stop dev server and commit if any fixes were needed**

```bash
# Only if Step 2 revealed issues requiring file fixes:
git add -A
git commit -m "fix: <describe what was wrong>"
```

If no fixes needed, skip this step.