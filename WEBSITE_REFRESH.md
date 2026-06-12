# FitPaws Website — UI Refresh (fitpaws.app)

**Created:** 2026-06-12
**Repo:** `fitpaws-website` (this repo) → GitHub `rkay1985-cyber/fitpaws-website` → Cloudflare Pages → https://fitpaws.app
**Goal:** A modern, conversion-focused landing page that promotes the app for **both Apple and Android**, and looks *amazing* on a phone — where most visitors will be.

**Status legend:** 🔴 Not started · 🟡 In progress · 🟢 Done

---

## Investigation findings (current state)

The site is a clean 5-page static build (`index/support/privacy/terms/confirmed.html` + one `styles.css`, no framework, no build step). Good bones: a tidy CSS-variable palette that matches the app's brand greens, sensible sections (hero / trust bar / features / how-it-works / pricing / CTA), and a working mobile hamburger. But:

1. **Every download button is a dead link.** All 6 CTAs are `href="#"` — including "Download on the App Store" in the hero, pricing, and CTA strip. The app is live (ASC app ID `6777413939`) and the site never links to it. **This is the single highest-value fix on the entire site.**
2. **iOS-only everywhere.** "Free to download on iOS" (meta + trust bar), Apple-logo-only buttons, no Google Play presence — at odds with the Android launch in progress (`fitpaws/ANDROID_SUPPORT.md`).
3. **Zero motion.** No `@keyframes`, no scroll reveals, only a single `.15s` hover transition. Sections pop in statically — same "flat/retro" feel as the app had.
4. **No real product imagery.** The hero "app preview" is a hand-built CSS mockup (fake ring, fake stats, dated "4 Jun 2025"). No screenshots of the actual app anywhere on the site.
5. **No social/SEO layer at all:** zero Open Graph / Twitter Card tags (links shared in chat show as bare text), no JSON-LD structured data, no `apple-itunes-app` smart banner, no canonical/sitemap.
6. **System font stack** (`-apple-system, …`) — fine, but contributes to the "default" look; no display typeface for headings.
7. **Assets hotlinked from Supabase storage** (logo + favicon load from `rtjvqugksbtsewtkbiih.supabase.co`) — extra DNS hop, a dependency on the app's backend for the marketing site, and no caching control.
8. **Mobile is functional, not great:** only two breakpoints (900/600px), no sticky mobile CTA, hero mockup pushes content far down on small screens, trust bar wraps awkwardly.
9. Small staleness: `© 2025` footer, "4 Jun 2025" in mockup, inline styles on `.price-annual`.

---

## Tracker

| # | Item | Effort | Status |
|---|------|--------|--------|
| WEB‑1 | Fix dead CTAs → real App Store link (+ Play placeholder) | XS | 🟡 dead links removed; **both badges greyed "coming soon"** (apps in TestFlight/dev) — swap to live store URLs at launch |
| WEB‑2 | Dual-platform: official store badges + copy for iPhone & Android | S | 🟡 badge-style buttons + copy done; swap to *official* badge artwork when links go live |
| WEB‑3 | Hero redesign: real screenshots in a device frame + typography | M | 🟡 display font (Plus Jakarta Sans) + layered gradient done; screenshots/device frame await WEB‑7 |
| WEB‑4 | Motion: scroll reveals, hero ring animation, card hovers | S–M | 🟢 hero entrance + staggered reveals + reduced-motion gate (ring animation deferred to WEB‑3 hero) |
| WEB‑5 | Mobile-first pass: sticky CTA, breakpoints, tap targets | M | 🔴 sticky CTA deliberately deferred until store links are live (sticky "coming soon" = anti-UX) |
| WEB‑6 | SEO/social: OG + Twitter cards, JSON-LD, smart banner, sitemap | 🟡 | OG/Twitter/JSON-LD/canonical done; smart banner + sitemap + designed share image pending |
| WEB‑7 | Screenshot pipeline: capture, frame, optimise (webp) | S | 🔴 |
| WEB‑8 | Content refresh: dates, FAQ, ratings/social-proof slot | S | 🟡 dates/inline-style/CTA anchors done; FAQ + social-proof slot pending |
| WEB‑9 | Performance/assets: self-host logos & images, Lighthouse pass | S | 🟡 logos/favicon self-hosted on all 5 pages; font still via Google CDN; Lighthouse pass pending |

> **Branch `website-refresh`, commit `2f632a6` — NOT merged/pushed.** Merging to the default branch deploys to production via Cloudflare Pages. Preview locally (`python3 -m http.server` in the repo) or push the branch for a Pages preview deployment first.
>
> **At launch:** replace both greyed badges with linked official artwork — Apple `https://apps.apple.com/app/id6777413939`, Play `https://play.google.com/store/apps/details?id=com.rlj.fitpaws` — re-enable WEB‑5's sticky CTA, and add the `apple-itunes-app` smart banner.

**Effort:** XS ≈ minutes · S ≈ <½ day · M ≈ ~1 day.
**Recommended order:** WEB‑1 immediately (it's a revenue leak) → WEB‑7 (screenshots unblock the hero) → WEB‑3 + WEB‑4 (the "modern" transformation) → WEB‑5 → WEB‑2 fully (Play badge goes live when the store listing exists) → WEB‑6, WEB‑8, WEB‑9.

---

## WEB‑1 · Fix the dead CTAs 🔴 — *do first, 10 minutes*

- All 6 `href="#"` → `https://apps.apple.com/app/id6777413939` (verify the final marketing URL from App Store Connect once the listing is public).
- Until the Play listing exists: second button shows the official Google Play badge greyed with "Coming soon to Android" (WEB‑2 makes it live later). Honest + signals Android intent today.
- `confirmed.html` deep-link page: check its store fallback link too.

## WEB‑2 · Dual-platform promotion 🔴

- Replace the hand-rolled Apple SVG button with the **official store badges** (Apple "Download on the App Store" + Google "Get it on Google Play") side-by-side in hero **and** CTA strip — the instantly-recognised pattern users scan for. Use the official badge artwork (Apple/Google brand guidelines: don't restyle them).
- Copy sweep: meta description + trust bar "Free to download on iOS" → "Free on iPhone and Android"; pricing footnote "your device's subscription settings" already platform-neutral ✓.
- When the Play listing goes live (ANDROID_SUPPORT.md AND‑1/2), drop in the real `https://play.google.com/store/apps/details?id=com.rlj.fitpaws` URL.

## WEB‑3 · Hero redesign 🔴 — *the "modern" centerpiece*

- **Real screenshots in a CSS device frame** (phone outline, rounded corners, subtle tilt/shadow) replacing the fake CSS mockup. Ideally two overlapping phones — home rings + AI coach — angled like contemporary app sites.
- **Display typeface for headings** — a single variable font (e.g. *Plus Jakarta Sans* or *Inter Display*), self-hosted (WEB‑9), `font-display: swap`; body can stay on the system stack.
- Richer hero background: layered radial gradients / soft mesh in the brand greens + a faint paw-print pattern, replacing the flat linear gradient.
- Tighten the headline (current 3-line break is wordy) and put both store badges directly under it, above the fold on mobile.

## WEB‑4 · Motion 🔴

Static site = a few dozen lines of CSS/JS, no library:

- **Scroll reveals** via `IntersectionObserver` toggling a class: features, steps, and price cards fade-up in with a slight stagger.
- **Hero ring animates** on load: `stroke-dashoffset` keyframe (SVG ring inside the device frame or rebuilt mockup) + count-up numbers — mirrors the app's planned UIX‑2 hero moment, nice brand echo.
- Card hover lifts (`translateY(-4px)` + shadow) on features/pricing; smooth-scroll for anchor nav.
- All wrapped in `@media (prefers-reduced-motion: no-preference)`.

## WEB‑5 · Mobile-first pass 🔴 — *"amazing on a phone"*

- **Sticky bottom CTA bar on mobile**: slim bar with both store badges that appears after scrolling past the hero — the single biggest mobile-conversion pattern the site lacks.
- Rework breakpoints (add ~768px and ~400px): hero stacks with the device frame *peeking* (not full height) so copy + badges stay above the fold; trust bar becomes a 2×2 grid; steps go full-width cards.
- Tap targets ≥44px (nav links and footer links are currently tight); hamburger menu gets a slide/fade transition.
- Test matrix: iPhone SE width (375), standard (390–430), small Android (360), tablet (768) — Safari iOS + Chrome Android.

## WEB‑6 · SEO & social layer 🔴

- **Open Graph + Twitter Card** tags with a designed 1200×630 share image (logo + device frame + tagline) — currently a shared link renders as plain text.
- **JSON-LD** `SoftwareApplication`/`MobileApplication` schema (name, OS, price range, store URLs; aggregate rating once reviews exist).
- `<meta name="apple-itunes-app" content="app-id=6777413939">` → native iOS Safari smart banner.
- Canonical tags, `sitemap.xml`, `robots.txt`; proper favicon set (`apple-touch-icon`, 32/16, mask icon) self-hosted.

## WEB‑7 · Screenshot pipeline 🔴

- Capture from the iOS simulator (home with filled rings, log flow, coach, progress) on a clean demo account with believable data ("Buddy", realistic kcal) — *after* the app's UIX‑2 ring animation lands they'll look even better, but don't block on it.
- Process: crop to device, export `webp` (+`png` fallback), 2× resolution, `loading="lazy"` below the fold; store in this repo under `/assets/`.
- Re-use these for the App Store/Play listings and the OG share image — one capture session, three uses.

## WEB‑8 · Content refresh 🔴

- `© 2025` → 2026 (or auto-year via JS); kill the dated "4 Jun 2025" with the real-screenshot hero.
- Add a short **FAQ** section (is it free? does it work for puppies? multiple dogs? cancel anytime?) — conversion + long-tail SEO.
- Reserve a **social-proof slot** (App Store rating stars / short review quotes) — populate once ratings accumulate; hide until then.
- Move the inline `.price-annual` styles into the stylesheet.

## WEB‑9 · Performance & assets 🔴

- **Self-host the logo/favicon** (currently hotlinked from Supabase storage — backend dependency + no cache control on the marketing site). Put them in `/assets/` with long-cache headers (`_headers` file for Cloudflare Pages).
- Subset + self-host the heading font (woff2, `preload`).
- Budget: Lighthouse ≥95 across the board — easily achievable for a static page if images are webp and lazy.

---

## Deployment notes

- Cloudflare Pages auto-deploys from the GitHub repo — pushing to the default branch is a **production deploy**. Do the refresh on a branch and use a Pages preview deployment to check mobile rendering before merging.
- No build step exists (plain HTML/CSS). Keep it that way — this refresh needs zero frameworks; a static page with good CSS is faster than anything a bundler would produce.

## Out of scope

- Blog/content marketing, email capture, analytics tooling — separate decisions.
- App Store / Play **listing** assets (screenshots/copy for the stores themselves) — overlaps WEB‑7's captures but tracked with the Android work.
