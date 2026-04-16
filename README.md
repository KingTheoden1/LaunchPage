# FormFlow — SaaS Landing Page

A production-quality, high-converting landing page for **FormFlow**, an accessible embeddable form builder with Stripe payments. Built as a portfolio piece to demonstrate SEO engineering, WCAG 2.1 AA accessibility compliance, and marketing conversion patterns in a single deployable artifact.

**Live demo:** https://formflow.app  
**Stack:** HTML5 · CSS3 · Vanilla JS · Tailwind CSS CDN · Vercel

---

## What this demonstrates

This project is intentionally a single `index.html` file — no framework, no build step. That choice is itself a demonstration: clean semantic markup is better for SEO crawlability and Figma import than React-generated HTML, and inlined critical CSS eliminates render-blocking requests that frameworks often reintroduce.

---

## SEO techniques

Every technique below is visible and named in the source code.

### Meta tags
- `<title>` — keyword-targeted, under 60 characters
- `<meta name="description">` — benefit-led, under 155 characters
- `<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large">` — explicit crawl directives
- `<link rel="canonical">` — prevents duplicate content penalties

### Open Graph (og:*)
Full suite: `og:type`, `og:site_name`, `og:title`, `og:description`, `og:url`, `og:image` (with `og:image:width`, `og:image:height`, `og:image:alt`)

### Twitter Card
`summary_large_image` card with `twitter:site`, `twitter:creator`, `twitter:image:alt`

### JSON-LD structured data (`@graph` with two schemas)
- **`WebSite`** — enables Google Sitelinks Search Box in SERPs
- **`SoftwareApplication`** — enables rich results: star ratings, price, OS, feature list

### Heading hierarchy
Single `<h1>` in hero → `<h2>` per section → `<h3>` per feature/plan — logically structured for crawlers and screen readers alike.

### Semantic HTML5 elements
`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<blockquote>`, `<cite>`, `<figure>`, `<figcaption>`, `<details>`, `<summary>`, `<address>`, `<footer>` — every element chosen for meaning, not just layout.

### sitemap.xml
- 12 URLs with `<loc>`, `<lastmod>`, `<changefreq>`, `<priority>`
- Priority tiered: homepage 1.0 → product 0.8 → supporting 0.6 → legal 0.4
- Declared in `robots.txt` for self-discovery without Search Console submission

### robots.txt
- `Allow: /` for all legitimate crawlers
- Admin, API, dashboard, and UTM query-string paths blocked (prevents duplicate content)
- AI training bots explicitly blocked: `GPTBot`, `Google-Extended`, `CCBot`, `anthropic-ai`, `Bytespider`

### Core Web Vitals
- **LCP** — hero image eager-loaded (not lazy), font preloaded with `<link rel="preload">`
- **CLS** — hero mockup height driven by content (no `aspect-ratio` constraint causing reflow)
- **FID/INP** — all JS deferred, non-render-blocking; Tailwind CDN loaded with `defer`
- Critical above-the-fold CSS inlined in `<style>` — eliminates render-blocking stylesheet request

### Performance
- `<link rel="preconnect">` to Google Fonts (DNS + TLS prewarmed)
- `<link rel="preload" as="style">` for font stylesheet (eliminates FOIT on hero text)
- Inline SVG favicon — zero extra network request
- All JS at end of `<body>` with no `async`/`defer` conflicts

---

## WCAG 2.1 AA techniques

Verified with axe-core 4.9.1 — **0 violations, 41 passing rules**.

### Perceivable
| Criterion | Implementation |
|---|---|
| 1.1.1 Non-text Content | Every `<img>` and `role="img"` has descriptive `alt` / `aria-label`; decorative SVGs have `aria-hidden="true" focusable="false"` |
| 1.3.1 Info & Relationships | Semantic elements convey structure: `<nav>`, `<main>`, `<article>`, `<blockquote>`, `<cite>`, `<ul>`, `<address>` |
| 1.3.3 Sensory Characteristics | No instruction relies on shape, color, or position alone — e.g. "Most Popular" badge uses text, not just a colored border |
| 1.4.1 Use of Color | Star ratings use `aria-label="5 out of 5 stars"` — rating not conveyed by color alone |
| 1.4.3 Contrast (AA) | All text meets 4.5:1 minimum. Key ratios: `#4F46E5` on white = **8.6:1**; `#111827` on white = **16:1**; white on `#4F46E5` = **8.6:1** |
| 1.4.4 Resize Text | All font sizes use `rem`/`clamp()` — scales correctly to 200% zoom |
| 1.4.10 Reflow | Flexbox with `flex-wrap` — no horizontal scroll at 320px width |

### Operable
| Criterion | Implementation |
|---|---|
| 2.1.1 Keyboard | Every interactive element reachable and operable by keyboard alone; `<details>/<summary>` natively keyboard-operable; mobile menu closes on `Escape` |
| 2.1.2 No Keyboard Trap | Mobile menu `Escape` handler returns focus to trigger button |
| 2.4.1 Bypass Blocks | Skip-to-content link is the first focusable element; jumps to `<main id="main-content">` |
| 2.4.3 Focus Order | DOM order matches visual order — no CSS-only reordering that breaks tab sequence |
| 2.4.6 Headings & Labels | Descriptive headings at every section; icon-only links have `aria-label` |
| 2.4.7 Focus Visible | Global `:focus-visible` ring — 3px solid `#4F46E5`, 3px offset — on every interactive element |
| 2.3.3 Animation | `prefers-reduced-motion` media query disables all transitions and animations |

### Understandable
| Criterion | Implementation |
|---|---|
| 3.1.1 Language of Page | `<html lang="en">` |
| 3.2.1 On Focus | No context changes on focus |
| 3.3.2 Labels or Instructions | All form inputs (mockup) have associated `<label>` elements |

### Robust
| Criterion | Implementation |
|---|---|
| 4.1.1 Parsing | Valid HTML5 — no duplicate IDs, properly nested elements |
| 4.1.2 Name, Role, Value | Mobile nav button: `aria-expanded`, `aria-controls`, `aria-label` updated on toggle; ARIA landmarks on every major region |
| 4.1.3 Status Messages | N/A — no dynamic status messages |

### ARIA landmarks
`role="banner"` · `role="navigation"` (×5, each `aria-label`-ed uniquely) · `role="main"` · `role="contentinfo"` — full landmark map for screen reader users

---

## Marketing / conversion patterns

- **Hero** — headline + sub-headline + primary CTA + secondary CTA + trust badge + 3 social proof stats
- **Testimonials** — 3 cards with star rating, quote, name, role, initials avatar; featured card in brand color
- **Features** — 8 feature blocks with coloured icon, heading, description; two visual rows
- **Pricing** — Free vs Pro comparison with "Most Popular" badge, feature checklist, CTAs, money-back guarantee
- **FAQ** — 6 accordion items using native `<details>/<summary>` (no JS required, Google-crawlable)
- **Pre-footer CTA band** — gradient full-width conversion strip before footer
- **Footer** — brand, social icons, 3 nav columns, legal links, auto-updating copyright year

---

## Figma import

This file is structured for clean import via the **HTML to Design** plugin (by div·RIOTS):

- Every section has an explicit `id` attribute → clean Figma layer naming
- Flexbox throughout (not CSS Grid) → maps to Figma Auto Layout
- No CSS custom properties on inline styles → consistent token extraction
- All colors, font sizes, and spacing consistent → Figma extracts design tokens correctly
- Single self-contained `index.html` with all styles in `<style>` block → no broken external references

**Recommended import settings:** Use Autolayout ✓ · Create styles & variables ✓ · HTML layer names ✓

---

## Project structure

```
LaunchPage/
├── index.html        # Complete single-file landing page
├── sitemap.xml       # Full sitemap, submit to Google Search Console
├── robots.txt        # Crawl rules + AI bot blocking
└── README.md         # This file
```

---

## Local development

```bash
# Serve locally with Node
npx serve . --listen 3000

# Or with Python
python -m http.server 3001
```

---

## Deploy to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy from project root
vercel

# Deploy to production
vercel --prod
```

No `vercel.json` config needed — Vercel detects a static site automatically and serves `index.html` as the root.

---

## Git branch structure

Each page section was built on its own branch and merged into `main`:

| Branch | Content |
|---|---|
| `main` | Foundation: meta tags, JSON-LD, document skeleton |
| `section/features` | Features section |
| `section/pricing` | Pricing section |
| `section/faq` | FAQ accordion |
| `section/footer` | Pre-footer CTA + footer |
| `section/seo-files` | sitemap.xml, robots.txt, axe audit fixes |
| `section/readme` | This README |
