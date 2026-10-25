# param — portfolio

Personal site of Paramveer Singh. Astro 7, zero client framework, one hero animation.

## Stack

- **Astro 7** — static output, content collections for work + writing
- **Fonts** self-hosted via Fontsource (Inter Variable, JetBrains Mono Variable)
- **Hero dragon** — 4,800 particles on a plain 2D canvas, one parametric formula, zero libraries. 30fps, pauses off-screen via IntersectionObserver, static frame under `prefers-reduced-motion`, auto-morphs between four presets every 12s (click to morph).
- **Page transitions** — native cross-document View Transitions (`@view-transition`), shared-element morph from work row to project page. Instant navigation where unsupported.
- **⌘K command menu** — instant open (keyboard-initiated, never animates), arrow-key navigation, client-side filter.
- **Theme** — dark default, light variant, circular wipe via View Transitions, one soft synthesized click sound (skipped under reduced motion).
- **RSS + sitemap** built in.

## Develop

```sh
npm install
npm run dev       # http://localhost:4321
npm run build
npm run preview
```

`?theme=light` forces the light theme for testing.

## Deploy

Static output in `dist/`. Cloudflare Pages: build command `npm run build`, output dir `dist`. Set the production domain in `astro.config.mjs` (`site`) so sitemap/OG URLs are correct.

## Content

- `src/content/work/*.md` — projects (`featured: true` puts them on the home page)
- `src/content/writing/*.md` — posts
