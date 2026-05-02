# riyaadseecharan.com

Personal site for Riyaad Seecharan — Miami-based healthcare technology entrepreneur and civic technologist (Pilot Manager at the Miami-Dade Innovation Authority, CTO of ELMC Rx Solutions, Baptist Health Knight Innovation Fellow).

**Live:** https://riyaad.seecharan.com (GitHub Pages, custom domain via `CNAME`)

---

## Tech stack

Single-file static site. No build step, no framework, no dependencies.

- HTML5 + inline `<style>` and `<script>`
- Google Fonts: Playfair Display (serif headlines), DM Sans (sans body), JetBrains Mono (mono labels)
- Inline SVG icons (Lucide-style, stroked, ISC license)
- Hosted on GitHub Pages from `main` branch root

To preview locally, just open `index.html` in a browser. No server required.

---

## File structure

```
.
├── index.html          # Entire site (HTML + CSS + JS)
├── README.md           # This file
├── CNAME               # GitHub Pages custom domain pointer
├── favicon.svg         # Browser tab icon (RS. monogram, deep navy + coral)
├── og-image.svg        # Source for social preview (design-of-record)
├── og-image.png        # 1200×630 PNG used by og:image (LinkedIn, X, iMessage)
├── robots.txt          # Allow all + sitemap pointer
└── sitemap.xml         # Single-URL sitemap
```

To regenerate `og-image.png` from `og-image.svg`:

```powershell
npx --yes svgexport og-image.svg og-image.png 1200:630
```

---

## Brand system

### Color palette (CSS custom properties in `:root`)

| Token | Hex | Use |
|---|---|---|
| `--coral` | `#E8634A` | Primary brand accent (decorative, large text, buttons) |
| `--coral-text` | `#C0432B` | Coral for small text where AA contrast matters |
| `--teal` | `#1F7268` | Secondary accent + small text (passes AA on cream) |
| `--gold` | `#E9C46A` | Decorative only on light bgs; text only on `--deep` |
| `--blush` | `#F4A89A` | Tertiary decoration |
| `--deep` | `#1A1A2E` | Headlines, dark sections |
| `--cream` | `#FDF8F2` | Default page background |
| `--sand` | `#F5EDE3` | Alternate section background |
| `--slate` | `#4A4A5A` | Body text |

### Typography

- **Playfair Display 900** — h1, h2, h3, logo, stat numbers
- **Playfair Display 400 italic** — accent ems inside headlines (in `--coral`)
- **DM Sans 300/400/500** — body, subtitles
- **JetBrains Mono 500** — eyebrows, section labels, timeline year markers

### Visual motifs

- Numbered section labels: `001 / Background`, `002 / The Arc`, etc.
- Colored left-border on stat blocks (rotates coral / teal / gold / blush)
- Colored top-bar `::after` on venture cards (same rotation)
- Decorative orbit graphic in hero (3 rings + 3 nodes + center pill)
- Lucide-style stroked SVG icons (`stroke-width="1.6"`, `currentColor`)

---

## Accessibility — WCAG 2.2 AA

This site targets WCAG 2.2 AA conformance. Specifically:

- **Skip link** as first body child (`a.skip-link`, off-screen until focused)
- **`<main id="main">`** wraps all content sections
- **`<nav aria-label="Primary">`** on the nav landmark
- **`aria-labelledby`** on every `<section>`
- **Focus styles** (`:focus-visible` outline) on every interactive element
- **`prefers-reduced-motion: reduce`** disables orbit spin and reveal transitions
- **`<noscript>`** fallback so reveal-hidden content stays visible without JS
- **Color contrast** verified for all text — `--teal` was darkened from `#2A9D8F` to `#1F7268` to pass 4.5:1 on cream; `--coral-text` exists for small-text contrast
- **Decorative emojis/icons** wrapped with `aria-hidden="true"`
- **`<span lang="fr">Café</span>`** for foreign-language word
- **Touch targets** — mobile nav links use 24×24+ tap area via padding

### Editing safely

- Don't use raw `--coral` or `--gold` for small text on light backgrounds — use `--coral-text` for coral, and use `--teal`, `--slate`, or `--deep` instead of gold.
- Don't use `--gold` for text anywhere except on `--deep` background.
- Any new interactive element must define a `:focus-visible` style (or rely on the global `a:focus-visible` rule).

---

## SEO / AEO

The site is built to be both crawlable by traditional search engines and quotable by AI engines (ChatGPT, Claude, Gemini, Perplexity).

**`<head>` includes:**
- `<title>`, `<meta name="description">`, canonical URL, robots
- Open Graph (`og:type=profile`, title, description, url, image, image dimensions, locale, profile:first_name/last_name)
- Twitter Card (`summary_large_image`)
- Favicon (`<link rel="icon" type="image/svg+xml">`)
- `theme-color` for mobile browser chrome
- Font preconnect for faster Google Fonts loading

**Three JSON-LD blocks:**
1. **`Person`** — name, url, image, jobTitle (array), homeLocation, worksFor (4 orgs), founder (5 orgs), alumniOf (4 orgs incl. NASA + Microsoft), knowsAbout (9 topics), sameAs (LinkedIn)
2. **`WebSite`** — name, url, author, language
3. **`FAQPage`** — 4 high-likelihood AI prompts: "Who is Riyaad Seecharan?", "Who founded Tesser Health?", "What does the Pilot Manager at Miami-Dade Innovation Authority do?", "What companies has Riyaad Seecharan founded or led?"

When updating titles, roles, or company facts, update the corresponding fields in the JSON-LD blocks too — they're authoritative for AI-engine citations.

---

## Email obfuscation

The contact CTA's email never appears as a contiguous string in the page source. The `mailto:` link and visible text are assembled at runtime from `data-u` / `data-d` / `data-t` attributes via JS. Bots that don't execute JS get a `[at]` / `[dot]` fallback. To change the email, update the `data-*` attributes on the `.contact-email` element (around line 1191) — there is no plain-text email anywhere else in the source.

---

## Common edits

| To change | Edit |
|---|---|
| A current title (e.g., MDIA role) | Update both the role chip in `.roles-strip` AND the `worksFor` array in the Person JSON-LD AND the venture card AND any FAQPage answer that mentions it |
| Add a new venture | New `.venture-card` block; add to `founder` array in Person JSON-LD; consider new FAQPage entry |
| Add a new community item | New `.community-item` block in `#community` |
| Update an icon | Replace inline `<svg>` with another Lucide icon path; size via parent CSS, color via `currentColor` |
| Add a `sameAs` profile | Append URL to `sameAs` array in Person JSON-LD |
| Update bio for AI quoting | Edit `description` in Person JSON-LD AND the "Who is Riyaad Seecharan?" answer in FAQPage |
| Replace social preview image | Edit `og-image.svg`, then re-run the svgexport command above |

---

## Changelog — May 2026 audit + redesign pass

Comprehensive review across SEO, accessibility, narrative, content, brand, frontend, and AI-citation visibility, followed by a 10-task fix pass.

### What changed

1. **Contact section added** — new `005 / Contact` section before footer with primary CTA (email, JS-obfuscated) and secondary LinkedIn button. The site previously had no contact path.
2. **Timeline gap filled** — new `2007–2013 · Between Acts` node added covering startup work, the launch of Not Going to Lie (2011), and the founding/exit of PxSource → Revolution EHR (2013). PxSource was previously misattributed to the 2014–Present block.
3. **SEO + AEO foundations** — added `<title>`, meta description, canonical, robots, Open Graph (8 tags + profile sub-properties), Twitter Card, favicon, theme-color, font preconnect, and three JSON-LD blocks (Person, WebSite, FAQPage). New files: `favicon.svg`, `og-image.svg`, `og-image.png`, `robots.txt`, `sitemap.xml`.
4. **WCAG 2.2 AA fixes** — `--teal` darkened from `#2A9D8F` (2.74:1 fail) to `#1F7268` (5.23:1 pass); new `--coral-text` for small-text contexts; global `:focus-visible` styles; skip link; `<main>` landmark; `aria-label` on nav; `aria-labelledby` on every section; `aria-hidden` on decorative emojis/icons; `<span lang="fr">` on Café; `prefers-reduced-motion` block; `<noscript>` reveal fallback; mobile touch targets; footer copy contrast 2.40:1 → 5.46:1; community paragraph opacity 0.6 → 0.78; speaking-tag color 0.65 → 0.82.
5. **Heading hierarchy fixed** — `.section-title` divs converted to proper `<h2>` (5 of them); community `<h4>` items promoted to `<h3>`. Outline now: 1 h1 → 5 h2 → 13 h3.
6. **Narrative through-line committed** — "the connector" spine now appears in hero eyebrow, hero subtitle, About title, About paragraphs, Ventures title, and Community title. Previously the site stated three competing theses ("public purpose", "where I build", "giving back").
7. **Editorial copy pass** — killed clichés ("civic plumbing", "at the seam", "bridging the gap", "serial healthcare technology entrepreneur turned public innovator", "next generation of builders and thinkers"). All 4 community cards and 2 weakest venture cards (Baptist Health, Seecharan Group) rewritten with real, specific facts (years, programs, focus areas).
8. **Voice converted to first-person** — Hero subtitle, About paragraphs, and Contact lede now in first-person ("I serve as Pilot Manager…"). Timeline retained in third-person as historical record.
9. **OS-rendered emojis replaced** with Lucide-style inline SVG icons (8 total: 3 orbit nodes, 4 venture v-icons, 1 footer pin). Brand now renders identically across Windows, macOS, iOS, Android.
10. **Mobile hero fixed** — orbit graphic was previously `display: none` on `<768px`. Now scales to 60% via CSS transform and renders below the hero text.

### Files added

- `favicon.svg` — RS. monogram (deep navy + coral period)
- `og-image.svg` — Design source for the social preview
- `og-image.png` — 1200×630 PNG referenced by `og:image` (rendered via `npx svgexport`)
- `robots.txt` — Allow all + sitemap pointer
- `sitemap.xml` — Single-URL sitemap

### Known TODOs / unverified claims

- Stat block numbers (`$81M+ Venture Value Created`, `5+ Companies Founded`, `5 Successful Exits`) were carried forward unchanged — not independently audited.
- EyeCheq financials (`$8M Series A`, `$40M valuation`) carried forward unchanged.
- Speaking tags (SIAA Charlotte, CoMotion Miami, Health 2.0, AARP Innovation 50, Global Wellness Institute, Ignite Miami) are inert `<div>`s with no event-page links — converting to anchor links would help AI-citation.
- Only LinkedIn is in the `sameAs` array. Adding Crunchbase, X, GitHub, MDIA team page, etc. is the highest-leverage AEO improvement available.
- No press / news section exists. Adding 3-5 outbound links to articles, podcasts, or panels would significantly improve AI-citation likelihood for unbranded queries.
