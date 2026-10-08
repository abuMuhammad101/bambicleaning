# bambicleaning.com — Site Anatomy (pre-redesign audit)

Captured **2026-10-08** from the live site (https://www.bambicleaning.com) by rendering every public route in headless Chromium at 1440×900 (desktop) and 390×844 (mobile), walking the interactive flows, and reading the production JS/CSS bundles.

Full-page screenshots of every page and key states are in [`screenshots/`](./screenshots).

---

## 1. Sitemap & routes

| Route | Purpose | Indexed | Notes |
|---|---|---|---|
| `/` | Home | yes (sitemap 1.0) | |
| `/services` | Service detail listing | yes (0.8) | |
| `/quote-calculator` | 4-step quote → booking → deposit payment | yes (0.7) | Nav label "Get a Quote" |
| `/contact-us` | Contact form + map | yes (0.8) | |
| `/privacy-policy` | Legal | yes (0.5) | |
| `/terms-and-conditions` | Legal | yes (0.5) | |
| `/payment/{fulfilled,pending,rejected}` | Post-payment result screens | no | |
| `/booking/…`, `/manage-booking/:id` | Customer booking management | disallowed | Reached via "Track my booking" (Booking ID + OTP) |
| `/admin/sign-in`, `/admin/forgot-password`, `/admin/bookings`, `/admin/bookings/:id`, `/admin/pending-cancellations`, `/admin/cancelled-bookings`, `/admin/support` | Admin back office | disallowed | Admin Portal is linked publicly in the footer |
| `*` / `/404` / `/error` | 404 & error pages | — | `/sign-in` and `/forgot-password` (listed in robots.txt) actually 404 |

`robots.txt` disallows `/sign-in`, `/forgot-password`, `/manage-booking/`, `/admin/`. `sitemap.xml` lists the 6 public pages (lastmod 2025-09-16).

---

## 2. Global elements

### Header (sticky, white, 64px)
- **Logo** (left): line-drawn deer + "Bambi" wordmark with letter-spaced "CLEANING" → `/`
- **Desktop nav** (≥ lg): Home · Get a Quote · Services · Contact Us — active item is bold
- **"Track my booking"** pill button (outlined, right) → opens a modal
- **Mobile** (< lg): logo · phone link `(734) 360-6176` · hamburger. The menu is a full-screen white drawer: Home, Get A Quote, Services, Contact Us (each with an icon), plus a "Track My Booking" link. ([screenshot](./screenshots/mobile-menu.png))
- There is no "Book Now" CTA in the desktop header, and the phone number only appears in the header on mobile.

### "Track My Booking" modal ([screenshot](./screenshots/track-booking.png))
- Title: *Track My Booking*. Helper text: "Enter your Booking ID & OTP to track your booking."
- Fields: `Booking Id` (number), `OTP` (number)
- Buttons: Cancel · Find → `/manage-booking/:id`

### Footer (off-white `#FFFDFA`, top border)
- Column 1: logo, tagline *"Graceful like a deer, thorough like a pro—Bambi Cleaning cleans high and low."*, Instagram icon → instagram.com/bambicleaning
- Column 2 **Links**: Home, Services, Contact Us, Admin Portal (`/admin/sign-in`). "Get a Quote" is not listed.
- Column 3 **Contact Info**: admin@bambicleaning.com · (734) 360-6176 · 2500 Packard St Suite 201C Ann Arbor MI
- Bottom bar: "Copyright © 2026 Bambi Cleaning" | "All rights reserved | Terms and Conditions | Privacy Policy"

---

## 3. Page-by-page anatomy

### 3.1 Home `/` ([desktop](./screenshots/home-desktop.png) · [mobile](./screenshots/home-mobile.png))
The page background is a faint, tiled cleaning-icon pattern (`/images/home/background.png`) under a 92% white overlay. The page is about 3,760px tall on desktop.

1. **Hero**: rounded (20px) full-width card with a photo (cleaner holding a mop in a white kitchen) and a dark overlay.
   - H1 *"Your Trusted Cleaning Partner!"* (Japandi 64px, white)
   - Sub: *"We handle the mess, so you can stress less."*
   - CTA **Book Now** (tan pill) → `/quote-calculator`
2. **Our Services**: eyebrow "Our Services" + heading "Cleaning Services Designed for You"
   - Carousel: 3 cards visible on desktop, 1 on mobile, with prev/next circular arrows. Each card is a photo with a dark overlay, H2 title, one-line description, and a **Learn More** button → `/services`
   - 6 services: Residential · Short-Term Rental · Office · Shared Space · Move-In/Move-Out · After-Construction
3. **Why Choose Us?**: eyebrow + heading "Why We're the Cleaning Team You Can Trust". There are 4 outlined cards with sage-green icons:
   - Experienced Professionals — Trained, vetted, and insured cleaners.
   - Advanced Cleaning — Powerful, safe, and designed for a healthier home.
   - Customized Plans — Tailored to fit your schedule and needs.
   - Satisfaction Guaranteed — We don't stop until you're happy.
4. **About Us / Who We Are**: photo (gloved hand vacuuming upholstery) on the left; on the right, a paragraph about the dedicated team, eco-friendly products and satisfaction. The **Learn More** button does nothing (no navigation).
5. **Locations / "Where We Provide Our Services"**: a single SVG image of a Michigan map with a "Southeast Michigan" pin, plus an "Areas we serve" card listing Sterling Heights, Troy, Farmington Hills, Bloomfield Hills, Rochester Hills, Royal Oak, Birmingham, Ann Arbor, Belleville ("All cities in the greater Detroit metro area"). **All of this text is drawn inside the SVG**, so it isn't real text: it can't be read by search engines or screen readers, and it's unreadably small on mobile.
6. Footer.

There are no testimonials or reviews, pricing teaser, FAQ, trust badges, stats, or before/after photos.

### 3.2 Services `/services` ([desktop](./screenshots/services-desktop.png) · [mobile](./screenshots/services-mobile.png))
1. **Banner header**: rounded image banner, H1 "Cleaning Services" (Japandi 42px, white)
2. **Carousel**: the same 6-card carousel as on the home page. Its Learn More buttons scroll down the page.
3. **Six detail blocks**, each with an image, an H3 in sage green (Japandi 30px), 4 bullet points, and a **Book Now** button → `/quote-calculator`:

| Service | Bullets cover |
|---|---|
| Residential | routine cleaning; on-demand deep clean; add-ons (upholstery, oven/fridge, inside windows, eco products); weekly/bi-weekly/monthly/one-off; insured, checklist-driven |
| Short-Term Rental | full turnover checklist (linens, beds, restock, trash); kitchen/bath reset; staging for photos & damage reporting; laundry/linen coordination |
| Office | workstation care; common areas & conference rooms; restroom/breakroom sanitation; daily/nightly/weekend plans |
| Shared Space | high-touch sanitization; floor care; appearance upkeep; frequency by foot traffic |
| Move-In / Move-Out | full deep clean incl. appliances & tracks; bathroom intensive; final touch-ups; checklist & photos for deposit inspections |
| After-Construction | rough clean; fine clean (drywall dust, vents); specialty (adhesive, paint splatter, glass); final walkthrough sign-off |

There is no pricing, FAQ or individual service pages: everything sits on one long page.

### 3.3 Quote Calculator `/quote-calculator` ([desktop](./screenshots/quote-calculator-desktop.png) · [mobile](./screenshots/quote-calculator-mobile.png))
The layout is a top progress bar ("Step name · n/4 Steps"), a centered large sage logo, and a "Get Help" link. The form is on the left and a sticky **summary card** (sage `#606C5A` background) is on the right. On mobile the summary collapses into a bottom bar.

**Summary card:** Beds · Bathrooms · Clean Type · Addons (count) · Frequency · Scheduled Date · **TOTAL** · PAYABLE (20% deposit) · BALANCE

| Step | Title | Inputs |
|---|---|---|
| 1 · Home Information | "Tell Us About Your Property" | **Clean Type** dropdown: Residential / Short-Term Rental / Commercial / Post-Construction / Renovation. **Bedrooms** and **Bathrooms** steppers (− 03 +), defaulting to 3 / 2. Choosing Commercial or Post-Construction hides the steppers and shows: "Please contact our support at **(734) 757-3603** or request a quote…" ([screenshot](./screenshots/qc-type-Commercial.png)) |
| 2 · Addons | "Addons" | "Select All Addons (13 items)" plus 13 checkbox cards: Inside Fridge $25 · Inside Oven $25 · Inside kitchen cabinets & drawers $25 · Baseboard Dusting $50 · Window Interior $50 · Garage Organization & Sweep $50 · Indoor Trash Cans $30 · Outdoor Trash Cans $50 · Patio/Balcony Refresh $25 · Light Fixtures & Ceiling Fans $25 · Linen Closet Organize $15 · Pantry Organize $20 · Welcome Basket Prep $30 ([screenshot](./screenshots/qc-step2.png)) |
| 3 · Schedule | "Tentative Schedule" | **Frequency** dropdown: One-Time / Weekly / Bi-Weekly / Monthly. **Date** picker (mm/dd/yyyy; no past dates). **Preferred Time**: big Hour : Minute tiles plus an AM/PM toggle ([freq](./screenshots/qc-freq.png), [date](./screenshots/qc-datepicker.png)) |
| 4 · Personal Information | "Personal Information" | Full Name · Email · Address · "Appartment, Suite, etc." (typo) · Zip Code · **Entry Method** (Someone is home / Door code / Lockbox / Key under mat) · **Pets** (No pets / Dog / Cat + pet name) · Special Instructions (optional, 500 chars max) → **Pay Now** |
| → Payment | — | 20% deposit is paid off-site with an external payment provider, then the user returns to `/payment/fulfilled`, `/payment/pending` or `/payment/rejected` |

Prices seen: Residential 3 bed/2 bath one-time = **$150** (deposit $30 / balance $120). Short-Term Rental 3/2 = **$175**.

**Get Help** opens an FAQ accordion ([screenshot](./screenshots/get-help-open.png)):
- How many steps? (4)
- Can I customize? (add-ons)
- How is the total calculated? (size, type, frequency, add-ons)
- How is payment made? (off-site, 20% deposit)
- What are Cleaning Types?
- Home Information

### 3.4 Contact Us `/contact-us` ([desktop](./screenshots/contact-us-desktop.png) · [mobile](./screenshots/contact-us-mobile.png))
1. Banner header: H1 "Contact Us" on the left and "(734) 360-6176" on the right (both part of the same H1)
2. Left: form with Name, Email, Subject, Your Message (0/500 counter) and a **Submit** button. Success message: "Your message has been sent to the admin successfully…"
3. Right: a **static map image** (`/images/contact-us/map.avif`) that shows **Wasena / Old Southwest, Roanoke, VA**, not Ann Arbor, MI.

There are no business hours, no address block, no clickable email or phone on the page body, and no live map embed.

### 3.5 Privacy Policy & Terms ([privacy](./screenshots/privacy-policy-desktop.png) · [terms](./screenshots/terms-and-conditions-desktop.png))
- Each has a plain H1 followed by H2 sections and body text.
- **Privacy** sections: GDPR compliance, Third-Party Websites, Information We Collect, Your rights, Changes. It is boilerplate and EU-focused (GDPR) for a Michigan business.
- **Terms** sections: 14 numbered sections. Business-relevant facts:
  - payment is due at booking;
  - the company may reschedule;
  - the customer must provide access and secure valuables;
  - cancelling less than 24 hours ahead incurs a **$75 fee**.

### 3.6 404 ([screenshot](./screenshots/404-desktop.png))
Illustration, "Oops! Page not found" (sage), and helpful links: Home | Contact Us | Services | Privacy Policy | Terms & Conditions.

---

## 4. Visual design system (as built)

### Typography
| Role | Font | Notes |
|---|---|---|
| Headings, logo-adjacent, buttons | **Japandi** (Regular 400 / Bold 700, self-hosted woff2) | Rounded geometric display face. H1 64px (home), 42–46px (inner), H2 26px (cards), 18px eyebrows |
| Body, nav, UI | **Futura Bk** (400, self-hosted) | |

### Color tokens (from CSS `:root`)
| Token | Value | Use |
|---|---|---|
| `--primary` | `#606C5A` (rgb 96 108 90) sage/olive | Service titles, quote summary card, progress bar, icons |
| `--yellow` | `#DFBF90` (rgb 223 191 144) tan/sand | **All primary CTAs** (pill buttons, black text) |
| `--yellow-dark` | `#C79F65` | hover |
| `--off-white` | `#FFFDFA` | Footer background |
| `--gray-dark` / `--gray-darker` | `#252525` / `#111111` | Body text / headings |
| `--gray` | `#3B505A` | Secondary text |
| `--gray-lighter` / `--gray-lightest` | `#DDDDDD` / `#F1F1F1` | Borders, fills |
| Eyebrow grey | `#737373` | "Our Services", "Why Choose Us?" labels |
| Utility | blue `#0062CC`, green `#11A849`, orange `#F5A623`, red `#EE0000` | Status chips (admin), toasts |

### Shape & components
- Buttons: fully rounded pills; the primary is tan with black text and the secondary is an outlined pill
- Cards & banners: 20px radius, image with dark overlay and white text
- Inputs: 1px grey border, pill-shaped (fully rounded) text fields
- Icons: Lucide (outline) plus solid sage feature icons
- The brand mark is a fine-line deer illustration, which ties to the "graceful like a deer" tagline

---

## 5. Tech stack & SEO/analytics

- **React SPA** built with Vite, Tailwind CSS v4, React Router, Redux Toolkit / RTK Query, react-hook-form + Zod, Lucide icons, and Helmet-style per-route meta tags.
- **Client-side rendering only**: the HTML the server returns is an empty `<div id="root">`, so all content depends on JavaScript. That's risky for SEO and for link previews.
- Images are AVIF. The service photos are up to **4096×2731 (~290 KB each)** and are displayed at about 450px wide.
- **Analytics:** Google Ads tag `AW-17539640233` only. There's no GA4, no Search Console verification tag, and no Meta pixel.
- **Meta:**
  - Every page has a title and description.
  - Canonical URLs use `www.`.
  - OG/Twitter image is `/og/home.avif`. AVIF isn't supported by Facebook, LinkedIn or iMessage previews, so those previews may show no image.
- **Structured data:** `Organization` (logo → `/logo.png`, which returns the SPA HTML, i.e. it doesn't exist) plus `BreadcrumbList`. There's **no `LocalBusiness`/`HouseCleaningService` schema**: no address, phone, hours, area served or reviews.

---

## 6. Issues & redesign opportunities

### Bugs / inconsistencies (fix regardless of redesign)
1. The **Contact map shows Roanoke, VA** instead of 2500 Packard St, Ann Arbor, MI.
2. There are **two phone numbers**: (734) 360-6176 everywhere except the quote calculator, which says (734) 757-3603.
3. Home **About → "Learn More"** goes nowhere.
4. Home service card "Learn More" lands on /services but not at the matching service.
5. The **service-area list is baked into an SVG** (invisible to SEO and screen readers, illegible on mobile).
6. `/logo.png` in the schema is a 404. The OG image is AVIF (poor social-card support).
7. Typo "Appartment". The Contact H1 merges the title and phone number into one heading.
8. robots.txt disallows `/sign-in` and `/forgot-password`, which don't exist; the real paths are under `/admin/`.
9. The service-photo carousel is duplicated on Home and Services.
10. Mobile footer bottom bar wraps awkwardly ("All rights reserved | Terms and Conditions | Privacy Policy").

### Content gaps
- No **reviews/testimonials**, star ratings or Google review widget
- No **pricing transparency** outside the calculator (e.g. "from $150")
- No **service-area pages** for local SEO (Ann Arbor, Troy, Royal Oak…)
- No **FAQ** page (the FAQ only lives in the calculator's "Get Help")
- No **hours**, no real team/"who we are" photos or story, no insurance/bonding proof, no satisfaction-guarantee details, no cancellation policy surfaced near booking
- No dedicated per-service pages (needed for SEO and ads landing pages)
- Instagram is the only social channel. There's no Google Business Profile link.

### UX opportunities
- Put **Book Now / Get a Quote** as a persistent header CTA and click-to-call on desktop
- Show an instant price estimate on the home page (beds/baths → price) to feed into the calculator
- Calculator:
  - show the price per add-on, plus frequency discounts if any;
  - surface the $75 cancellation policy before Pay Now;
  - allow Commercial/Post-Construction requests through a form instead of "call us";
  - add a ZIP-code service-area check early.
- Contact page: a live Google Map embed, address, hours, and a clickable phone and email
- Move the Admin Portal link out of the public footer
- Server-side rendering / prerendering (or a static-site build) for SEO

---

## 7. Asset inventory

| Asset | Path |
|---|---|
| Fonts | `/fonts/japandi/japandi-{regular,bold}.woff2`, `/fonts/futura-bk/futura-bk-regular.woff2` |
| Hero / page backgrounds | `/images/home/background.png` (pattern), `/images/banners/services.avif`, `/images/banners/contact-us.avif` |
| Service photos | `/images/services/{residential,short-term-rental,office,shared-space,move-in-out,after-construction}.avif` |
| About photo | `/images/home/about-us.avif` |
| Locations map | `/images/home/service-locations.svg` |
| Contact map | `/images/contact-us/map.avif` (wrong city) |
| 404 illustration | `/images/layout/404.svg` |
| OG image | `/og/home.avif` |
| Favicon | `/favicon.ico` |
