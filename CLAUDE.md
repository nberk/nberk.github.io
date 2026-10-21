# nberk.github.io

Personal site for Nick Berk, deployed at [nick-berk.com](https://nick-berk.com).

## Stack

- **Static site generator**: Hugo 0.158.0 extended
- **Hosting**: GitHub Pages via GitHub Actions
- **Custom domain**: `nick-berk.com` (CNAME in `static/CNAME`)

## Project Structure

```
hugo.toml              # Hugo configuration
content/               # Markdown content (homepage, blog posts)
layouts/               # HTML templates (baseof, partials, index)
assets/css/style.css   # Site-wide styles (design tokens + all components)
static/fonts/          # Self-hosted Newsreader woff2 (roman + italic)
static/                # Static files (favicon, headshot, CNAME)
docs/                  # Design/implementation notes
.github/workflows/     # GitHub Actions deploy workflow
```

## Design

Visual identity is **"The Reading Room"**: a clean near-white page, a literary book
serif, reading-lamp green links. See [docs/reading-room-redesign.md](docs/reading-room-redesign.md).

- **Typeface**: [Newsreader](https://fonts.google.com/specimen/Newsreader) (SIL OFL),
  **self-hosted** from `static/fonts/` as a variable woff2 (roman + italic, latin subset).
  No third-party font requests — declared via `@font-face` at the top of `style.css`
  and preloaded in `layouts/partials/head.html`.
- **Theming**: **light only** by design. All colors are CSS custom properties on `:root`
  in `style.css`; `color-scheme: light` forces light browser UI even in dark-mode
  environments. There is no `prefers-color-scheme` override. Style components through the
  tokens, never hard-coded colors.
- **Templates reference class names** (`.intro`, `.homepage-section`, `.post-content`,
  `.lessons-content`, `.table-wrap`, …); `style.css` re-themes those hooks, so restyling
  rarely needs template edits.
- To change the font: swap the woff2 in `static/fonts/`, update the `@font-face` `src`
  and `--serif` stack, and the `preload` href in `head.html`.

## Commands

```bash
# Local dev server
hugo server -D

# Build site
hugo --minify
```

## Deployment

Push to `main` triggers `.github/workflows/hugo.yml`, which builds with Hugo and deploys to GitHub Pages. No manual deploy step needed.

GitHub Pages source is set to **GitHub Actions** (not branch-based).
