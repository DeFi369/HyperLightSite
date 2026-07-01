# HyperLightSite — Audit & Work Log

**Repo:** HyperLightTech storefront — a warm-charcoal / persimmon landing page selling **$9 DIY guides** (Linux hardening, DeFi, AI agents) with a $39 Flagship / $49 Master ladder and a custom-build upsell.
**Local path:** `~/HyperLightSite` · **Remote:** `github.com/DeFi369/HyperLightSite` (origin/main, DeFi369 GH account).
**Live:** only at **https://defi369.github.io/HyperLightSite/** (GitHub Pages, source = `main` `/`). The apex `hyperlighttech.com` does **NOT** resolve — the Pages `cname` is null, so the custom domain is not wired up.
**Stack:** plain static site, **no build step**. Hand-written HTML with one inline `<style>` + one inline `<script>` per page, self-hosted fonts. Deploy = serve the files.

## File layout
- `index.html` — the whole landing page (inline `<style>` + inline `<script>`).
- `thank-you.html` — post-purchase page (`noindex`).
- `404.html` — themed not-found page.
- `og-image.html` — 1200×630 source that `og-image.png` is rendered from (headless chromium screenshot).
- `og-image.png` — social share card (on-brand, "$9", `.com`).
- `favicon.svg` — bolt mark.
- `sitemap.xml`, `robots.txt` — SEO/crawler basics.
- `fonts/` — self-hosted webfonts: `fonts.css` + 7 `.woff2` (Manrope 400/500/600/700, Syne 600/700/800), latin subset only. Referenced by all 4 HTML files.
- `README.md` — guides-first project readme.
- `AUDIT.md` — this file (canonical status).
- Gumroad: `hyperlighttech.gumroad.com/l/{linux-guide,defi-guide,ai-agent-guide,guide-bundle}`.

## Current index.html section flow (real ids, in order)
`hero` (no id) → `#guides` ($9 cards ×3 + $19 Core Bundle line + 30-day guarantee + hidden free-sample line) → `#flagship` (dark band, $39 Flagship + $49 Master, both "Coming soon" until URLs set) → `#how` (why builders buy) → `#build` (custom-build CTA, mailto) → `#faq` (7 items) → `#updates` (Follow on Gumroad). Then footer.

## Structured data / JS
- 3 JSON-LD blocks: Organization (with $9/$19/$39/$49 offers), Product graph (3 guides), FAQPage (7 Q&As, mirrors the visible FAQ).
- Inline `<script>`: `toggleMenu`/`closeMenu`/`toggleFaq` (a11y-aware), the `FLAGSHIP_URL`/`MASTER_URL`/`SAMPLE_URL` swap-point logic, and the Gumroad overlay-checkout binding. `gumroad.js` loaded `defer` at the end.

---

## Status

**2026-07-01 — `buttons-machined-keys` branch (this pass, pushed, NOT merged — awaiting owner review).** Button-system redesign + two pre-existing bugs found & fixed:
1. **New button language, all 3 pages ("machined keys"):** the friendly pills (`border-radius:999px`, persimmon glow) → squared 6px-radius keys with stamped uppercase Manrope-700 13px labels (`.11em` tracking), a solid hard rim (`box-shadow:0 4px 0`), hover lifts the key 1px, `:active` depresses it flush (rim collapses to 0 — a press you can feel). Variants: primary = persimmon key (charcoal text, keeps the 5.27:1 AA fix), ghost = charcoal key (card-bg + hairline border, warms to persimmon on hover), light = bone key (flagship band). Arrow SVGs nudge right on hover; `:focus-visible` outlines added; `prefers-reduced-motion` disables button transitions. Unused `.btn-ondark-ghost` removed.
2. **`.soon` = recessed socket:** the Flagship/Master "Coming soon" state is now an inset, dash-bordered empty socket — the slot exists, the key isn't installed yet.
3. **Gumroad re-skin bug (pre-existing, LIVE) fixed:** `gumroad.js` injects a runtime stylesheet that was re-skinning every `a.gumroad-button` — the 3 card buy buttons + bundle line rendered as BLACK Gumroad-branded buttons with an injected `.logo-full` wordmark, clipping the $9 price block. Added higher-specificity overrides (`a.btn.gumroad-button`, `.bundle-line a.gumroad-button`, …) that keep the overlay checkout but restore our styling; wordmark hidden via `a.gumroad-button span.logo-full{display:none}` (must out-specify their identical `a.gumroad-button .logo-full`). **Gotcha: gumroad.js redefines `--accent` on the element — overrides must use literal `#F0633F`, never `var(--accent)`.** Overrides also pre-cover the future flagship (`btn-light`) / master / sample gumroad links.
4. **Mobile-nav bug (pre-existing) fixed:** `.nav>.nav-cta{display:none}` in the ≤760px media query never matched (`<nav>` has no `nav` class), so the header CTA overflowed the viewport and pushed the hamburger off-screen → now `nav>.nav-cta`.
5. **thank-you/404 parity + contrast:** their `.nav-cta`/`.btn`/`.home` restyled to keys; leftover white-on-persimmon text (3.2:1 AA fail missed by the quick-wins pass) → `var(--paper)`.
Verified via headless-chromium renders (desktop 1440px, mobile 390px, thank-you, 404) with gumroad.js live during render.

**2026-06-30 — `storefront-quickwins` branch.** Eight targeted quick-wins:
1. **Truth fix:** guide-01 card desc + Product JSON-LD now say **DNS-over-TLS** (the guide configures DoT, not DoH). Zero `DNS-over-HTTPS` left.
2. **CTA hierarchy:** the 3 guide-card buy buttons `btn-ghost → btn-primary`, relabeled "Get it" → "Get the guide — $9" (the real buy buttons are now the loudest, not the quietest).
3. **Contrast (WCAG AA):** `.btn-primary` text `#fff → var(--paper)` (dark charcoal on persimmon) = **5.27:1** (was 3.2:1, failed). `--ink-3` `#8C8175 → #A89C8C` = **6.27:1** on charcoal (was 4.43:1). `--accent` untouched.
4. **Logo hrefs:** header + footer logos `href="#" → href="/"` (matches thank-you/404).
5. **A11y:** FAQ buttons got `aria-expanded` + `aria-controls`; panels got `id`s + `hidden`; `toggleFaq` toggles both and adds/removes `hidden` (via `transitionend` on close) so collapsed panels leave the a11y tree while the max-height animation still runs. Hamburger got `aria-expanded` (+ `aria-controls`), toggled in `toggleMenu`/`closeMenu`.
6. **FAQ:** added "What's your refund policy?" + "What format is it — can I read it on my phone?" to the visible FAQ **and** the FAQPage JSON-LD (7 Q&As total, in sync).
7. **Self-hosted fonts:** removed render-blocking Google Fonts (2 preconnects + the stylesheet `<link>`) from all 4 HTML files; added `fonts/` (Manrope 400/500/600/700 + Syne 600/700/800, latin-only `.woff2`, `font-display:swap`) + `fonts.css`, linked in each page. No third-party font fetch on any page (privacy-first). NOTE: Manrope and Syne are variable fonts, so the per-weight latin `.woff2` files are byte-identical within each family (one variable file serves all weights via the `font-weight` descriptor) — every declared family/weight still resolves to a local file that exists.
8. **Docs:** this AUDIT.md rewritten to current reality; README.md reframed guides-first.

**2026-06-29 — Redesign shipped + polished (on `main`, LIVE).** `index.html` is the warm-charcoal / persimmon refined-minimal redesign (~6 sections, was ~13); dropped Font Awesome CDN for inline SVGs; thank-you.html + 404.html rebuilt in the charcoal theme; `og-image.png` regenerated on-brand from `og-image.html`; Gumroad overlay checkout + 30-day guarantee + Product/FAQ JSON-LD added; `#updates` Follow-on-Gumroad band added for audience capture. All merged to `main` and live via Pages.

---

## Open (honest)

### Out of scope for this branch / other tracks
- **Apex domain dead + canonical still points at it.** `hyperlighttech.com` does not resolve, yet `<link rel=canonical>` / `og:url` / `og:image` all use `https://hyperlighttech.com/`. **OUT OF SCOPE here (domain track)** — either wire the custom domain to Pages or repoint canonical/OG to the live `defi369.github.io/HyperLightSite/` URL.
- **`FLAGSHIP_URL` / `MASTER_URL` / `SAMPLE_URL` are `''`** in index.html's script, so the $39 Flagship + $49 Master render "Coming soon" and the free-sample line stays hidden. Set them once the Gumroad products exist (other track — depends on the guides repo shipping the Flagship + its Gumroad listings).

### Still open (this storefront)
- **Zero social proof.** No testimonials, no counts, no logos — the earlier fabricated testimonials were (correctly) removed and nothing genuine replaced them. Add real proof when available (FTC-safe).
- **Product cover art not on the cards.** Branded guide covers exist in the guides repo (`~/hyperlighttech-guides`) but the storefront cards are text-only (numbered chips). Putting covers on the cards is a deliberate other track (do not do it here).
- **No email capture beyond "Follow on Gumroad."** The `#updates` band routes to Gumroad follow / mailto; a dedicated list (privacy-respecting) is the main lever for turning one-time buyers into repeat revenue.
- **No analytics** (deliberate — owner is privacy-first). If ever wanted, use Gumroad's built-in stats or a cookieless tool (GoatCounter/Plausible/Fathom), owner's OK required.
- **SEO blog** for the guide topics is still the biggest untapped growth lever.
