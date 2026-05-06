# Math On Canvas — Documentation

Documentation site for [Math On Canvas](https://www.math-on-canvas.com/editor) — a browser-based editor for creating mathematical posters and diagrams.

Built with [Astro](https://astro.build) + [Starlight](https://starlight.astro.build).

## Stack

- **Astro 6** + **Starlight 0.38**
- Languages: EN (root), RU — DE and ES declared, content added progressively
- Markdown / MDX — MDX only for pages with YouTube video embeds

## Project Structure

```
src/
├── assets/              # Logo and images
├── components/
│   └── starlight/       # Starlight component overrides
├── content/
│   └── docs/
│       ├── index.mdx               # EN homepage (splash)
│       ├── getting-started/
│       ├── guides/
│       │   ├── shapes-2d/
│       │   └── shapes-3d/
│       ├── reference/
│       └── ru/                     # RU translations
└── styles/
    └── theme.css        # Custom theme (orange accent, Geist font, grid bg)
docs/
└── superpowers/
    ├── specs/           # Architecture design docs
    └── plans/           # Implementation plans
```

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start dev server at `localhost:4321`        |
| `npm run build`   | Build production site to `./dist/`          |
| `npm run preview` | Preview production build locally            |

## Content

Pages are stubs — frontmatter and heading structure are in place, prose content is written separately. YouTube video IDs are placeholders (`YOUTUBE_*_ID`) in 4 pages:

- `guides/shapes-2d/circle.mdx`
- `guides/shapes-2d/rectangle.mdx`
- `guides/shapes-2d/triangle.mdx`
- `guides/shapes-2d/line-arrow.mdx`

Replace each placeholder with the real YouTube video ID before publishing.
