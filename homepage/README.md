# Bambi Cleaning — new homepage (v1)

A static, self-contained page with no build step. Open `index.html` directly, or serve the folder:

```sh
cd homepage && python3 -m http.server 8000   # → http://localhost:8000
```

Full-page previews: [`preview/desktop.png`](preview/desktop.png) · [`preview/mobile.png`](preview/mobile.png)

## Design basis
Contra's aesthetic, adapted to Bambi's brand, with selected Programa patterns. See [`../docs/homepage-references.md`](../docs/homepage-references.md).

- **Type:** Inter Tight for headings and body; Instrument Serif italic for one accent phrase per heading.
- **Colour:** sage `#606C5A` / deep sage `#2F3529` (dark surfaces), sand `#DFBF90` (primary CTA), off-white `#FFFDFA` canvas, sage-tint `#EEF0EA` band.
- **Components:** pill buttons and chips; 20–28px radius cards; iridescent "soap bubble" decoration in the hero.

## Sections
1. Sticky header with a "Book a clean" call-to-action, plus a full-screen mobile menu.
2. Hero with the instant-quote bar (type of clean · bedrooms · bathrooms) and "Popular" preset chips.
3. Three hero photos with floating status cards.
4. Trust row.
5. "Three easy steps", shown as UI vignettes of the real quote calculator.
6. Services as a photo grid of six tiles with starting prices.
7. Pricing facts: $150 typical clean, 20% deposit, 24h free cancellation, 13 add-ons.
8. Reviews in a masonry grid with result headlines.
9. About Bambi on a dark sage card.
10. Service areas: city chips plus a schematic map (all real text).
11. FAQ accordion, with answers from the current site's terms and calculator help.
12. Closing call-to-action over a photo.
13. Footer, with no Admin Portal link.

## Before launch
- **Reviews are placeholders.** Every card marked `data-placeholder` has sample text and "Customer name". Replace them with real, attributed reviews (e.g. from Google Business Profile). The star row also needs a real rating source.
- **Starting prices:**
  - Only Residential **$150** and Short-term rental **$175** (3 bed / 2 bath, one-time) are confirmed from the live calculator.
  - Move-in/out says "Instant quote"; Office, Shared spaces and After-construction say "Custom quote".
- **Quote bar hand-off:** it submits `?type=&beds=&baths=` to `/quote-calculator`. The current calculator ignores these parameters, so it needs a small change to pre-fill from them.
- **Track booking** links to `/manage-booking/`. The live site opens a Booking ID + one-time-code modal instead, so wire it to the same modal.
- **Phone:** standardised on (734) 360-6176. The live calculator still shows (734) 757-3603.
- **Images** in `assets/` are Bambi's existing photos, pulled from the live site. Swap in real team or job photos when you have them.
- **The map** is schematic (relative positions only), not to scale.
