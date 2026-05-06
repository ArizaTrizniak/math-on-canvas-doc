# Design Spec: Math On Canvas Theme for Documentation Site

**Date:** 2026-05-06  
**Status:** Approved

## Goal

Apply the visual style of the `math-on-canvas` Next.js app to the `math-on-canvas-doc` Astro/Starlight documentation site. Content is not touched — only styling and theme configuration.

## Constraints

- Light theme only (no dark mode)
- No component-level overrides (Starlight components stay as-is)
- Content files (`src/content/docs/**`) are not modified

## Approach: CSS Variables + Light-Only (Variant B)

Override Starlight's CSS custom properties via a single `src/styles/theme.css` file. Fonts loaded via `@fontsource-variable` packages (self-hosted, same approach as Next.js font optimization in math-on-canvas).

Dark theme neutralized by re-declaring the same light values under `[data-theme='dark']` and `[data-theme='dark'] ::backdrop` — the toggle button remains but has no visual effect.

## Dependencies to Install

```
npm install @fontsource-variable/geist @fontsource-variable/geist-mono
```

## Files Changed

### `astro.config.mjs`

- Change `title` from `'My Docs'` to `'Math On Canvas'`
- Add `customCss` array:
  ```js
  customCss: [
    '@fontsource-variable/geist',
    '@fontsource-variable/geist-mono',
    './src/styles/theme.css',
  ]
  ```

### `src/styles/theme.css` (new file)

Targets `[data-theme='light']` (Starlight's light mode selector) to set math-on-canvas colors and fonts. Then repeats the same block under `[data-theme='dark']` to neutralize dark mode.

**Starlight light-mode selector:** `:root[data-theme='light'], [data-theme='light'] ::backdrop`

**Accent colors (from math-on-canvas):**

| Variable | Value | Notes |
|---|---|---|
| `--sl-color-accent` | `#f97316` | Orange — active sidebar links, highlights |
| `--sl-color-accent-high` | `#ea6d0f` | Darker orange — active text color |
| `--sl-color-accent-low` | `rgba(249, 115, 22, 0.12)` | Active sidebar item background tint |

**Background colors:**

| Variable | Value | Notes |
|---|---|---|
| `--sl-color-bg` | `#ffffff` | Main content area |
| `--sl-color-bg-nav` | `#ffffff` | Top nav (Starlight default is gray-7 ≈ #f9fafb) |
| `--sl-color-bg-sidebar` | `#ffffff` | Sidebar (Starlight default inherits from bg) |

**Gray scale — Starlight inverts semantics in light mode:**  
In light mode, `--sl-color-white` = darkest shade (text), `--sl-color-black` = lightest (background).

| Variable | Value | math-on-canvas equivalent |
|---|---|---|
| `--sl-color-white` | `#111827` | Primary text |
| `--sl-color-gray-1` | `#1f2937` | Near-black text |
| `--sl-color-gray-2` | `#374151` | Dark gray text |
| `--sl-color-gray-3` | `#4b5563` | Medium gray text |
| `--sl-color-gray-4` | `#9ca3af` | Muted text |
| `--sl-color-gray-5` | `#d1d5db` | Subtle borders |
| `--sl-color-gray-6` | `#f3f4f6` | Light background tint |
| `--sl-color-gray-7` | `#f9fafb` | Near-white background |
| `--sl-color-black` | `#ffffff` | Page background |
| `--sl-color-hairline` | `#e5e7eb` | Dividers and borders |

**Typography:**

| Variable | Value |
|---|---|
| `--sl-font` | `'Geist Variable', system-ui, sans-serif` |
| `--sl-font-mono` | `'Geist Mono Variable', monospace` |

**Light-only enforcement:**

```css
/* Apply light theme overrides */
:root[data-theme='light'],
[data-theme='light'] ::backdrop {
  /* all variables above */
}

/* Neutralize dark mode — same values */
:root[data-theme='dark'],
[data-theme='dark'] ::backdrop {
  /* identical to light block */
}
```

Note: Starlight's `:root` defaults to dark theme. The initial page render will use Starlight's default dark colors for a brief moment before the theme class is applied by JS. This is a known limitation of the CSS-only approach and is acceptable for a documentation site.

## Color Source Reference (math-on-canvas)

- Primary accent: `#f97316` (`LandingPage.css`, active carousel dot, hover states)
- Gradient: `linear-gradient(135deg, #ffbe7a, #e86f1d)` (CTA buttons — reference only, not applied to Starlight)
- Borders: `#e5e7eb` (card borders)
- Text primary: `#111827`
- Text secondary: `#4b5563`, `#6b7280`
- Background: `#ffffff`, `#fafafa`

## Logo

Copy `logo.svg` from `math-on-canvas/public/images/logo.svg` to `src/assets/logo.svg` in the doc project (already done).

Configure in `astro.config.mjs` via Starlight's `logo` option:

```js
starlight({
  title: 'Math On Canvas',
  logo: {
    src: './src/assets/logo.svg',
    alt: 'Math On Canvas',
  },
  // ...
})
```

Starlight will display the logo in the top-left nav alongside the site title. The SVG is 128×128 with an orange ellipse, cyan triangle, and white graph line — matches the math-on-canvas brand.

## Out of Scope

- Sidebar structure or content changes
- Custom Starlight component overrides
- Social links or GitHub URL updates
- Gradient buttons (Starlight doesn't have CTA-style primary buttons)