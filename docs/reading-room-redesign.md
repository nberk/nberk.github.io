# The Reading Room — site redesign plan

Redesign of nick-berk.com adopting the **Reading Room** identity: a well-read
person's homepage on a clean near-white page, set in a literary book serif, with
writing given pride of place and a deep reading-lamp green for links.

Approved from the [five-directions pitch](https://claude.ai/code/artifact/a20cbf2e-530a-4c83-bd53-e9a5e8b6e343).

## Design system

### Type
- **One family, sized.** [Newsreader](https://fonts.google.com/specimen/Newsreader)
  (SIL OFL, open source) — a serif designed for on-screen reading. Roman 400/500/600
  + Italic 400. No separate sans; labels are the same face, small and letterspaced.
- Fallback stack: `"Newsreader", "Hoefler Text", "Iowan Old Style", "Palatino Linotype", Georgia, serif`.
- Reading-first scale (up from today's 15px):

  | role | size | line-height |
  |---|---|---|
  | body / post | 17px | 1.72 |
  | homepage lists | 16px | 1.55 |
  | post h1 | 2rem | 1.15 |
  | section label | 12px, `letter-spacing .12em`, uppercase, green | — |

- Descriptions and taglines set in *italic* (the essayist's aside).

### Color

| token | value | use |
|---|---|---|
| `--paper` | `#FCFCFB` | page background (near-white) |
| `--paper-2` | `#F1F0EC` | raised panels: code, table head, tags |
| `--ink` | `#262521` | body text |
| `--muted` | `#6C6960` | secondary text, dates, descriptions |
| `--green` | `#3C5A45` | links, section labels (the reading lamp) |
| `--green-deep` | `#2F4838` | link hover |
| `--rule` | `#E7E5E0` | hairline rules, borders |
| `--rule-strong` | `#D7D4CC` | stronger borders, list markers |

Neutrals are near-white with only a whisper of warmth, chosen rather than a dead grey.

### Font hosting (privacy note)
Today the site pulls Inter from `fonts.googleapis.com` — every visitor's browser
hits Google. We'll **self-host** Newsreader instead: download the latin-subset
`.woff2` files into `static/fonts/`, declare `@font-face` locally, and drop the
Google `<link>` + preconnects. Zero third-party font requests. Fits the
*Thoughts on Privacy* / Privacy Policy Analyzer side of the site.

### Theme
**Light only.** The warm-paper theme renders for every visitor regardless of their
OS setting. `:root` sets `color-scheme: light` so browser-drawn UI (form controls,
scrollbars) stays light in dark-mode environments too. No `prefers-color-scheme`
override, no toggle.

## Files touched

1. **`static/fonts/`** *(new)* — 2 self-hosted variable woff2: Newsreader roman + italic (latin subset).
2. **`layouts/partials/head.html`** — remove Google Fonts `<link>` + 2 preconnects; add a `preload` for the primary font. Everything else (counter.dev analytics, RSS, favicon, CSS pipe) untouched.
3. **`assets/css/style.css`** — full re-skin. Keep **every existing class hook** the templates use (`.site-header`, `.intro`, `.homepage-section`, `.group-label`, `.project-list`, `.post-list`, `.tag-badge`, `.post-content`, `.lessons-content`, `.table-wrap`, …) so most templates are untouched; only the blog list and tag pages got new markup (`.post-item`), and `term.html` folded into `taxonomy.html`. Re-theme through the tokens above; add the token block and the `@font-face` rules.
4. **`CLAUDE.md`** — update the Stack section: self-hosted fonts, the design-token system, light-only theme.

## Out of scope (no content changes)
- Homepage section order stays Writing → Projects → Resource Lists (already writing-first).
- `reads.nick-berk.com` and `locallaw.nick-berk.com` are separate sites.
- Post copy, front matter, and the counter.dev analytics tag are left alone.

## Build order
1. Fetch + place the woff2 files.
2. Rewrite `style.css` (tokens → base → header → homepage → blog → post → lessons → tables → responsive).
3. Update `head.html`.
4. Run `hugo server`, verify homepage + a blog post + lessons in the browser.
5. Update `CLAUDE.md`. Suggest a commit.
