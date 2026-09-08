# Dalkon Enterprises — site rebuild notes

## Files

```
index.html                                  the site (single page, no build step)
favicon.svg  apple-touch-icon.png           icons
icon-192.png  icon-512.png                  PWA / Android icons
site.webmanifest                            web app manifest
robots.txt                                  crawler rules + sitemap pointer
sitemap.xml                                 submit this to Google Search Console
assets/logo-mark-gold.svg                   true vector logo (~1 KB, was 220 KB)
assets/logo-mark-navy.svg
assets/logo-mark-white.svg
assets/og-image.png                         1200×630 social share card
assets/dalkon-enterprises-company-profile.pdf
```

Upload the whole folder to your web root. There is nothing to compile.

---

## Three things to do before it goes live

### 1. Wire up the contact form

Open `index.html`, find `FORM_ENDPOINT` near the bottom (in the `<script>` block).

Until you replace it, the form falls back to opening the visitor's email
app with everything pre-filled — so no enquiry is ever silently lost. But a
real endpoint converts far better.

Two free options:

- **Web3Forms** — sign up at web3forms.com, paste your access key into
  `WEB3FORMS_KEY`, and set `FORM_ENDPOINT` to `https://api.web3forms.com/submit`
- **Formspree** — set `FORM_ENDPOINT` to `https://formspree.io/f/YOUR_ID`

### 2. Confirm the canonical domain

Everything is set to `https://www.dalkonenterprises.com/`. If your live site
serves from the bare domain (no `www`), search-and-replace
`https://www.dalkonenterprises.com` throughout `index.html`, `robots.txt` and
`sitemap.xml`. Then make sure the other version 301-redirects to it — serving
the same page on both URLs splits your ranking signals in half.

### 3. Add your street address

The structured data currently declares Harare and Zimbabwe but has no
`streetAddress`, because your company profile doesn't list one and I wasn't
going to invent it. Find `"addressLocality": "Harare"` in the JSON-LD block and
add a `"streetAddress"` line above it.

---

## Off-page SEO — this is where the ranking actually comes from

The page is now technically clean. Technical SEO gets you *eligible* to rank;
these get you *ranked*. Roughly in order of impact for a local Harare business:

1. **Google Business Profile.** Free, and the single biggest lever for
   "web design Harare" type searches. Claim it at business.google.com. Use the
   exact same business name, phone and address as the website — Google
   cross-checks these, and mismatches cost you.
2. **Google Search Console.** Verify the domain, then submit
   `https://www.dalkonenterprises.com/sitemap.xml`. This is also where you'll
   see which queries you're actually appearing for.
3. **Bing Webmaster Tools.** Five minutes, and it feeds several other engines.
4. **Local directories.** Zimbabwe business listings, chambers of commerce,
   industry associations. Consistent name/address/phone everywhere.
5. **Client work as case studies.** Right now the Work section describes
   *categories* of work rather than named projects. Three real case studies
   with client names, the problem, and the outcome would do more for both
   ranking and conversion than any technical change left on this page.
6. **Backlinks.** A link from any reputable Zimbabwean site — a client's
   "built by" credit, a directory, a local news mention — is worth more than
   dozens of low-quality ones.

### Where to grow next

The biggest remaining structural limit is that this is a **single page**.
Single pages can only rank for one cluster of keywords. When you have the
content for it, separate pages for each service — `/web-design-harare`,
`/logo-design-zimbabwe`, `/custom-software-zimbabwe` — would let each one
target its own search terms. A blog answering the questions your clients
actually ask would compound over time.

---

## What changed from the previous version

**Performance**
- Both logo files were 220 KB PNGs wrapped in SVG. Retraced as real vectors
  (~1 KB each) and inlined into the HTML as an SVG symbol, so the mark now
  costs zero network requests and scales cleanly at any size. Verified at
  0.43% pixel difference from the original — antialiasing only.
- Fonts load non-blocking, trimmed from 11 weights to 6.
- Scroll handler throttled with `requestAnimationFrame` instead of firing on
  every scroll event.

**Bug fixes**
- `.wrap` had no `width`, so inside the flex hero it collapsed to content
  width and pushed the hero text ~250 px right of the nav logo. Fixed.
- The "Company Profile" button was hidden entirely on mobile, so phone
  visitors had no CTA in the header. Replaced with "Get a free quote".

**SEO**
- Title and description rewritten around search intent and location.
- Added canonical URL, Open Graph, Twitter cards, `lang="en-ZW"`, geo meta.
- JSON-LD structured data: ProfessionalService/Organization (with founders,
  full service catalogue, area served), WebSite, WebPage, FAQPage.
- Added `robots.txt`, `sitemap.xml`, web manifest.
- Content depth roughly 700 → 1,650 words. New Process section and an
  eight-question FAQ targeting long-tail queries ("how much does a website
  cost in Zimbabwe"). The FAQ schema makes it eligible for expanded results.
- Filenames hyphenated — spaces in URLs encode as `%20` and look broken when
  shared.

**Conversion**
- Real enquiry form: validation, spam honeypot, mailto fallback.
- WhatsApp CTAs throughout plus a floating button that appears on scroll.
- Scrollspy so the nav highlights the section being read.

**Accessibility**
- Skip link, `<main>` landmark, `aria-labelledby` on every section.
- Mobile menu: Escape to close, focus moves into the drawer on open and back
  to the toggle on close, backdrop scrim, animated hamburger.
- Darkened `--gold-deep` and `--slate` slightly to clear WCAG AA contrast.
- Full `prefers-reduced-motion` handling and a print stylesheet.
