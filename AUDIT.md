# HyperLightSite — Audit & Work Log

**Repo:** HyperLightTech marketing landing page (`$9 DIY guides` + custom-build upsell).
**Local path:** `~/HyperLightSite` · **Remote:** `github.com/DeFi369/HyperLightSite` (origin/main) · **Live:** hyperlighttech.com
**Stack:** plain static site, no build. Hand-written/minified HTML+CSS+vanilla JS. Deploy = serve files.

## File layout
- `index.html` — entire landing page (inline `<style>` + `<script>`). Sections: urgency-bar → nav → hero → stats → #resources (guides + bundle) → #how-it-works → #why-us → #services → #quiz → #tiers → #testimonials → #portfolio → #mid-cta → #faq → #book (mailto form) → footer + floating widgets.
- `thank-you.html` (post-purchase, noindex) · `404.html` · `robots.txt` · `sitemap.xml`
- `favicon.svg` · `og-image.png` · `guide-01/02/03-card.png` (product thumbnails) · `README.md`
- Gumroad: `hyperlighttech.gumroad.com/l/{linux-guide,defi-guide,ai-agent-guide,guide-bundle}`

---

## Status
**2026-06-28 (LATEST) — SITE REDESIGNED + SHIPPED LIVE.** `index.html` replaced with a warm-charcoal, refined-minimal redesign: 6 sections (was ~13), persimmon `#F0633F` accent (was cold cyan), Syne+Manrope kept, Font Awesome CDN dropped for inline SVGs. Cut quiz/portfolio/stats/testimonials/exit-popup/pill/floating-CTA/urgency-bar. `$39` Flagship + `$49` Master render a clean **"Coming soon"** until their Gumroad products exist (set `FLAGSHIP_URL`/`MASTER_URL` in index.html's script to make them live). The 3× `$9` guides + `$19` core bundle are real/live links. thank-you.html/404.html/robots.txt carry the `tek→tech` + favicon fixes (thank-you's placeholder "Level up" block removed). Committed → merged to `main` → pushed → **live via GitHub Pages** (defi369.github.io/HyperLightSite).
**OPEN:** re-theme thank-you.html + 404.html to the charcoal palette (still old navy + cyan); regenerate guide-card PNGs + og-image (baked-in `tek`/`.io`); flip Flagship/Master live once Gumroad products exist. (guide-0x-card.png now unused by index.html.)

---
**2026-06-28 (earlier, pre-redesign) — First full audit + P0 money-bug fixes (now superseded by the redesign for index.html; still relevant for the other pages/assets).**
Done this pass: reverted the broken uncommitted CTA restyle (back to known-good solid-cyan buttons); fixed every `hyperlighttek → hyperlighttech` occurrence (thank-you.html cross-sell links + support email ×3, 404.html footer, robots.txt sitemap); removed the dead `favicon.ico` links from index.html + 404.html (no ICO tooling on this box — the SVG favicon covers all current browsers). Verified: zero `tek`/`.io` left in site files, all Gumroad links on `hyperlighttech.gumroad.com`.

**Session 2 (same day): $49 Master-bundle + $39 Flagship scaffold BUILT in working tree (uncommitted).** index.html: added the Flagship band ($39) + Master bundle card ($49, new "Best Value", $66→$49 save $17) + a secondary $19 Core Bundle link in #resources; featured pricing tier → Master $49; mid-CTA → Master; quiz AI result upsells the Flagship for production-scale answers; Org schema offers for $39 + $49. thank-you.html: "Level up" Flagship/Master upsell. **Checkout links are PLACEHOLDERS via a single swap point** — `FLAGSHIP_URL` / `MASTER_URL` consts in the page script (currently `''`); buttons carry `data-checkout` + a dashed outline and an amber scaffold banner shows until BOTH are set, then everything auto-clears. Verified: JS `node --check` clean, brace/paren/bracket balanced, div balance 269/269.
**Remaining for launch:** (1) paste the 2 real Gumroad URLs into `FLAGSHIP_URL`/`MASTER_URL`; (2) write the Flagship guide itself (next task); (3) guide-04 card art (can be inline SVG); (4) soft copy still on the $19 core (og:description, #how-it-works guarantee, FAQ) — truthful as-is, could mention Master/Flagship; (5) still-open: `tek`/`.io` baked into the 3 PNGs + og-image redesign.

## Open findings (prioritized)

### P0 — Broken / revenue-losing
- [x] **(FIXED — reverted)** Broken uncommitted CTA restyle (dark bg + near-black text → invisible buttons). Reverted to last commit's solid-cyan buttons per owner.
- [x] **(FIXED)** `tek` → `tech` everywhere in site text: thank-you.html cross-sell links + support email, 404.html footer, robots.txt sitemap.
- [ ] **Wrong domain baked into images (needs regen, not text):** all 3 guide-card PNG watermarks + `og-image.png` show `hyperlighttek` / `hyperlighttech.io` (site is `.com`). STILL OPEN — needs image editing (no rasterizer on this box).

### P1 — Conversion / SEO / trust
- [x] **(FIXED)** Dead `favicon.ico` links removed from index.html + 404.html (SVG favicon remains). Still TODO (optional): a real root `/favicon.ico` for legacy/crawler hits, plus `apple-touch-icon` + `theme-color`.
- [ ] **og-image is off-message:** agency tagline ("Autonomous intelligence…"), no "$9 guides" — wasted social card for the guide push. Redesign to sell the guides; fix TLD.
- [ ] **Testimonials + portfolio metrics appear placeholder/unverified** (David K./Aisha L./Marcus T.; $4.2M TVL, 23% gain, zero incidents). FTC risk — confirm genuine or soften/remove.
- [ ] `sitemap.xml` lists the noindex thank-you page and has stale `lastmod` (2026-05-27).
- [ ] Org schema only; no per-guide `Product`/`Offer` schema (misses rich-result eligibility for the products being pushed).

### P2 — Performance / privacy / a11y
- [ ] Render-blocking third-party **Google Fonts + Font Awesome (cdnjs)** on all pages — self-host/subset to match privacy-first ethos and improve LCP (conversion + SEO).
- [ ] Low-contrast `--muted #6b7280` small text (guide includes, footnotes) likely fails WCAG AA.
- [ ] FAQ `<button>`s lack initial `aria-expanded`/`aria-controls`; mailto success screen has no fallback if the visitor has no mail client.

### P3 — Brand polish
- [ ] Three different brand marks (nav "HL" square, favicon bolt, og-image hexagon+bolt) — unify.
- [ ] `README.md` still agency-first; guides are now the primary offer — reframe.
- [ ] Guide-01 card art says "DNS-over-TLS" while the site bullet says "DNS-over-HTTPS" — pick one.

## Revenue / premium-guide opportunities
- **Pricing ladder gap:** $9 → $19 → [gap] → $2,500. A premium guide at **$39–$79** fills the first rung, raises AOV, and anchors the $19 bundle.
- **Recommended flagship:** premium AI-agents guide (multi-agent "Hermes" playbook) ~$49 with downloadable code repo + templates + deploy/observability + lifetime updates.
- **No email capture anywhere** (only the custom-build form) — the key missing lever for turning one-time buyers into repeat/recurring revenue. Add list capture; membership viable at ≥4–5 guides.
