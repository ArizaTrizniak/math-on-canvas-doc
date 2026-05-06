# Math Poster Editor — Overview for Documentation Agents

This document describes the Math Poster Editor application in enough detail for an agent to write
user-facing documentation (help articles, feature guides, API docs, or tooltips) without needing
to read the full source code. File paths are relative to `src/`.

---

## 1. What the Editor Is

**Math Poster Editor** is a browser-based canvas editor for creating mathematical posters and diagrams.
Users compose pages from shapes, text, LaTeX formulas, images, and 3D objects, then export to PDF, SVG,
or PNG. An optional AI panel can generate formulas or entire posters from a natural-language prompt.

**Stack:** React 19 · TypeScript · Vite · Konva.js 9.3 (canvas) · Zustand + Immer (state) ·
MathJax 3.2 + MathLive (LaTeX) · jsPDF + svg2pdf (export) · react-i18next (EN / RU / DE / ES)

---

## 2. Element Types

All elements are defined in `models/types.ts` and created via factories in `models/elementFactory.ts`.

### 2.1 Text
Plain text box. Properties: font family, font size, color, bold, italic, optional fixed width.

### 2.2 Formula
LaTeX-based formula rendered to SVG via MathJax. Properties: LaTeX string, font size, font family,
color, inline vs block display. Double-click on canvas opens the inline LaTeX editor
(`widgets/FormulaEditor/`). The editor can also be pre-populated from the Predefined Formulas palette
(`models/formulas.ts` — 60+ entries: algebra, geometry, trigonometry).

### 2.3 Shapes (2D) — 14 kinds
| Kind | Notes |
|---|---|
| `circle` | Perfect circle, optional center marker |
| `oval` | Ellipse |
| `rectangle` | |
| `triangle`, `pentagon`, `hexagon` | Regular polygons |
| `polygon`, `polyline` | Arbitrary vertex list |
| `parallelogram`, `rhombus`, `trapezoid` | |
| `arc`, `sector` | Circular arc / wedge |
| `arrow-line` | Line with configurable heads (none / arrow / triangle / circle / square / bar) |
| `page-frame` | Page boundary rectangle |
| `page-table` | Grid with configurable rows and columns |

Shape properties: stroke color, fill color, fill opacity, stroke width, dash pattern
(solid / dashed / dotted / dash-dot).

### 2.4 3D Shapes — 7 kinds
`cube` · `prism` · `pyramid` (3-sided) · `pyramid` (general) · `sphere` · `cone` · `cylinder`

Properties: width / height / depth dimensions, 3×3 rotation matrix, face opacity, optional vertex
labels, optional center point marker. The 3D projection is rendered via the software projection engine
in `canvas/elements/shapes/Shape3DView`.

### 2.5 Draw (Freehand)
Two sub-tools:
- **Pencil** — thin stroke, full opacity.
- **Marker** — thick stroke, semi-transparent.

Properties: stroke color, stroke width, opacity, line cap/join, tension.

### 2.6 Image
Raster image from a local file upload. Properties: x, y, width, height, aspect-ratio lock.

### 2.7 Annotation
Angle annotation with arc, square, or dot style. Displays an angle value with customizable font size.
Side annotations are also implemented.

### 2.8 Group
Recursive container. Multiple elements can be grouped/ungrouped via the Edit Property Bar or context menu.

---

## 3. Connection / Vertex System

Shapes can be spatially linked through *vertices* (reference points) and *connections*:

- **Enabling**: Turn on via the Vertex Settings button in the toolbar.
- **Snap tolerance**: Configurable radius (default 10 px); when an arrow-line endpoint or polygon vertex
  is dragged within that radius of another element's vertex, it snaps and connects.
- **Propagation**: Moving a shape moves all connected elements with it.
- **Spatial index**: `models/spatialIndex.ts` (GridSpatialIndex) provides O(1) approximate vertex lookup.

---

## 4. Document & Pages

- **Page formats**: A4, Letter, Poster (2× Letter). Portrait or landscape.
- Multi-page documents. Pages are listed in the Sidebar → Pages panel.
- Page operations: add, delete, duplicate, rename.
- Active page is the only page rendered in the canvas at one time.
- `models/pageFactory.ts` creates / clones pages.

---

## 5. UI Layout

```
┌─────────────────────────────────────────────────┐
│  ToolBar (top)                                  │
├────┬────────────────────────────┬───────────────┤
│ E  │                            │               │
│ d  │     Canvas (Konva.js)      │  SideBar      │
│ i  │                            │  (right)      │
│ t  │                            │               │
│ o  │                            │               │
│ r  ├────────────────────────────┤               │
│ B  │  EditPropertyBar (float)   │               │
│ a  │                            │               │
│ r  │                            │               │
└────┴────────────────────────────┴───────────────┘
   Palettes (draggable, anywhere on canvas)
   AI Panel (right side, toggled)
```

### 5.1 ToolBar (`widgets/ToolBar/`)
Left cluster — page controls:
- **Orientation** toggle (portrait ↔ landscape)
- **Page Format** selector (A4 / Letter / Poster)
- **Add Page Frame** — inserts page boundary rectangle
- **Add Page Table** — inserts grid table (rows/cols dialog)

Middle cluster — canvas tools:
- **Snap to Grid** toggle
- **Vertex Settings** — enable connections, set snap tolerance

Right of middle:
- **Undo** / **Redo** / **Delete**

Far right:
- **New Document** — resets to blank
- **Export** menu (PDF, SVG, PNG — page selection dialog)
- **Import / Export Document** (cloud, authenticated users only)
- **AI Panel** toggle
- **Language switcher**
- **About**
- **User Menu** (login / logout / profile)

### 5.2 EditorBar (`widgets/EditorBar/`) — vertical, left of canvas
Quick-insert buttons (48×48 px, `EditorBarButton`):
- Add Text
- Math Symbol Palette toggle
- Predefined Formulas Palette toggle
- Shape Palette toggle
- 3D Shape Palette toggle
- Add Image (file picker)
- Free Draw toggle (pencil / marker sub-modes)

### 5.3 SideBar (`widgets/SideBar/`) — right panel, resizable & collapsible
Three sections:

**Pages** — thumbnail list, add / duplicate / delete per page.

**Elements** — tree of all elements on the active page. Each row has:
- Name (type + index)
- Visibility eye icon
- Lock icon

**Properties** — context-sensitive panel driven by the selected element type:
- Text: font, size, color, bold/italic, width
- Shape: stroke color, fill color, opacity, dash pattern
- 3D Shape: dimensions, rotation matrix, face opacity, vertex labels
- All elements: position x/y, rotation, element ID, type

### 5.4 EditPropertyBar (`widgets/EditPropertyBar/`) — floating above canvas
Appears when an element is selected:
- **Single selection**: Rotation angle input + element-specific inline editor.
- **Multi-selection**: Group / Ungroup buttons.
- Lock / Unlock quick-toggle.
- Free Draw mode: stroke color and width inputs.

### 5.5 Palettes (`widgets/Palettes/`) — draggable floating panels
1. **Math Symbol Palette** — 150+ symbols (Greek, sets, relations, geometry, calculus, logic, arrows,
   constants) organized by category. Click inserts a formula element.
2. **Formula Palette** — 60+ predefined LaTeX formulas. Click inserts a formula element.
3. **Shape Palette** — all 14 2D shapes. Click inserts the chosen shape.
4. **3D Shape Palette** — all 7 3D shapes. Click inserts.
5. **Free Draw Palette** — pencil/marker color and width settings.

Positions are persisted in `localStorage` under key `palettePositions`.

### 5.6 Formula Editor Portal (`widgets/FormulaEditor/`)
Inline LaTeX editor that opens when a formula element is double-clicked on the canvas.
MathLive provides live preview. Enter confirms, Escape cancels.

### 5.7 AI Panel (`widgets/AI/`)
Toggled by the AI Button in the toolbar. Two tabs:

**Formula tab** — Enter a natural-language prompt (optionally with the current LaTeX for edits).
On success, a formula element is inserted / updated and added to undo history. Panel auto-closes
after 2 seconds.

**Document tab** — Enter a prompt. On success, the entire document is replaced in a single
batched undo step.

Job lifecycle: Submitting → Polling (queued → running) → Succeeded | Failed.
Max wait: 120 seconds. Cancellable. Errors classified as auth / network / timeout / unknown.

### 5.8 Native Auth Modal (`widgets/NativeAuthModal/`)
Email + password flows:
- **Sign In**: POST `/auth/sign-in` → HttpOnly cookie session.
- **Sign Up**: POST `/auth/sign-up` → may require email confirmation.
- **Confirm**: Code-based verification (POST `/auth/confirm`).
- Resend code option.

---

## 6. Canvas (`canvas/`)

**Stage**: `canvas/StageView.tsx` wraps a Konva Stage.

**Layers** (bottom to top):
1. Grid layer — optional background grid.
2. Content layer — all document elements.
3. Selection layer — drag-selection rectangle.
4. Vertex layer — connection point indicators.
5. Overlay layer — path tool preview, free draw preview.

**Interaction hooks:**
- `useStageEvents` — pan, zoom, element drag/drop.
- `useFreeDrawTool` — pencil / marker stroke recording.
- `usePathTool` — polygon path: click to add vertex, double-click or Escape to close.
- `useKeyboardShortcuts` — arrow-key nudge, Delete, Undo/Redo.
- `useSelectionRectangle` — drag-select multiple elements.

**Context menu** (right-click on element): Cut, Copy, Paste, Duplicate, Delete, Edit
(text/formula), AI Edit (for formulas).

---

## 7. State Management

The Zustand store is composed from 6 slices (`store/slices/`):

| Slice | Key state | Key actions |
|---|---|---|
| `documentSlice` | pages, active page, elements, vertices | addElement, deleteElement, updateElement, addPage, deletePage, duplicatePage, replaceDocument, beginBatch/endBatch |
| `selectionSlice` | selectedId, selectedIds | select, deselect, clearSelection |
| `historySlice` | undo/redo stacks (max 100) | _record, undo, redo, clearHistory |
| `uiSlice` | sidebar state, palette positions, draw settings, vertex settings | setSidebarCollapsed, setVertexSettings, … |
| `aiSlice` | panelOpen, activeTab, activeJob | ai_submitFormulaJob, ai_submitDocumentJob, ai_cancelJob |
| `authSlice` | authStatus, authUser, modal state | auth_init, auth_signIn, auth_signUp, auth_logout |

**Persistence** (key `math-poster-editor-v1`): Only the `doc` subtree is saved to `localStorage`.
Auth, history, and UI are ephemeral.

**Critical patterns for agents reading the code:**
- `selectedElement()` and `canUndo()` are *functions*, not values — always call with `()`.
- All mutations that should be undoable call `_record()`.
- `selectedId` and `selectedIds` must always stay in sync.
- `doc_new()` resets the document and clears history.

---

## 8. Export

| Format | Entry point | Notes |
|---|---|---|
| PDF | `pdf/saveToPdf.ts` | Elements → SVG → svg2pdf → jsPDF. Custom fonts (NotoSansMath, Roboto). |
| SVG | `svg/saveToSvg.ts` | Full vector. Multi-page → ZIP. |
| PNG | `png/saveToPng.ts` | Raster via canvas. Multi-page → ZIP. |

Export dialog allows selecting which pages to include. Authenticated users additionally have a
**cloud Import / Export Document** that syncs with the backend.

---

## 9. Internationalization

- Library: react-i18next with browser-language auto-detection.
- Languages: EN, RU, DE, ES.
- Namespaces under `i18n/locales/{lang}/`: `sidebar`, `toolbar`, `ai`, `auth`, and others.
- Validation: `npm run i18n:validate` checks for missing keys across all locales.

---

## 10. Notable Predefined Content

### Math symbols (`models/mathSymbols.ts`) — 150+ symbols
Categories: Greek letters, set theory, relations, geometry, arithmetic operators, calculus, logic,
arrows, constants.

### Predefined formulas (`models/formulas.ts`) — 60+ formulas
Categories: algebra (quadratic formula, binomial, logarithm rules …), plane geometry (area, perimeter,
Pythagorean theorem …), trigonometry (sin/cos/tan identities, law of sines/cosines …).

---

## 11. Auth & Permissions Summary

| Feature | Guest | Authenticated |
|---|---|---|
| Full canvas editing | ✓ | ✓ |
| All element types | ✓ | ✓ |
| PDF / SVG / PNG export | ✓ | ✓ |
| AI Formula generation | ✗ | ✓ |
| AI Document generation | ✗ | ✓ |
| Cloud Import / Export | ✗ | ✓ |

Auth is *fail-open*: network errors on session check → guest mode (non-blocking).

---

## 12. Key File Reference

| Topic | Location |
|---|---|
| Element type definitions | `models/types.ts` |
| Element factories | `models/elementFactory.ts` |
| Page factories | `models/pageFactory.ts` |
| 60+ predefined formulas | `models/formulas.ts` |
| 150+ math symbols | `models/mathSymbols.ts` |
| Geometry utilities | `models/geometry.ts` |
| Vertex / connection logic | `models/vertexManager.ts`, `models/connectionManager.ts` |
| Document store slice | `store/slices/documentSlice.ts` |
| UI store slice | `store/slices/uiSlice.ts` |
| AI store slice | `store/slices/aiSlice.ts` |
| Auth store slice | `store/slices/authSlice.ts` |
| Canvas main component | `canvas/StageView.tsx` |
| Element renderers | `canvas/elements/` |
| Shape renderers | `canvas/elements/shapes/` |
| ToolBar | `widgets/ToolBar/` |
| EditorBar | `widgets/EditorBar/` |
| SideBar | `widgets/SideBar/` |
| Palettes | `widgets/Palettes/` |
| Formula editor | `widgets/FormulaEditor/` |
| AI panel | `widgets/AI/` |
| Auth modal | `widgets/NativeAuthModal/` |
| PDF export | `pdf/saveToPdf.ts` |
| SVG export | `svg/saveToSvg.ts` |
| PNG export | `png/saveToPng.ts` |
| AI service (API calls) | `services/ai/` |
| Auth service + interceptor | `services/auth/` |
| i18n locale files | `i18n/locales/` |