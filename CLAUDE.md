# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev        # dev server on :4321
npm run build      # production build
npm run preview    # preview production build
```

## Architecture

Static single-page portfolio. **Astro 4 + Tailwind CSS**. No SSR, no framework components — pure `.astro` files.

### Routing & i18n

Two locales served as separate routes: `/es` and `/`. Root redirects to `/es`.

```
src/pages/
  index.astro          # redirects to /es
  es/index.astro       # Spanish page
  en/index.astro       # English page
src/i18n/
  es.ts                # all Spanish strings + data (talks, press items, stats)
  en.ts                # all English strings + data
```

Each page imports the relevant i18n object and passes it as `t` prop to every component. **All user-visible text and structured data (talks, press items, stats) lives in the i18n files**, not in components.

### Component pattern

Every section is a standalone `src/components/*.astro` receiving `t: any`. Sections are assembled in the page files with no shared state. Order in pages: Hero → About → Runnea → Speaking → Press → Personal → Contact+Footer.

Section numbering (01–06) is hardcoded in each component's header label.

### Styling

Tailwind only. Custom tokens defined in `tailwind.config.mjs`:
- `accent` / `accent-dark` — lime green (`#a3e635` / `#84cc16`)
- `surface` — `#111111` (card backgrounds)
- `muted` — `#a3a3a3` (secondary text)

Dark-first design (black background). No CSS files.

### Images

All images in `public/images/`. Referenced with root-relative paths (`/images/filename.jpg`). No Astro image optimisation — plain `<img>` tags.

Photo naming convention:
- `avatar.png`, `running.jpg` — Hero section
- `about-jorge.jpg` — About section
- `runnea-logo.png` — Runnea section
- `speaking-*.jpg` — Speaking event photos
- `logo-*.png` — Press outlet logos
- `press-*.png` — Press clipping screenshots
- `personal-*.jpg` — Personal section photos

### Contact form

`Contact.astro` submits to **Web3Forms** (`https://api.web3forms.com/submit`) via `fetch`. Access key is hardcoded in the hidden `access_key` input. Emails go to `jgbetanzos@gmail.com`.

### Deployment

Vercel, auto-deploys on push to `main`. No build configuration needed beyond the defaults.
