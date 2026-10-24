# Frontend Architecture & Implementation Recommendations

> **Document Version:** 1.0.0  
> **Target Project:** Grand Pasundan Convention Hotel Redesign  
> **Scope:** Architecture Strategy, Framework Evaluation, Integration Architecture, and Roadmap  
> **Author:** Senior UI/UX Designer & Frontend Architect  
> **Constraint Check:** No framework initialized; no application code written. Planning and technical specifications only.

---

## 1. Technical Evaluation of Frontend Frameworks

To replicate the ultra-luxurious, smooth feel of Schlosshotel Kitzbühel while maintaining high performance and SEO visibility in the Indonesian and international travel market, four architecture paradigms were evaluated:

| Criterion | **Astro (Island Architecture)** | **Next.js (App Router / SSR)** | **Vite + React / Vue (SPA)** | **WordPress Headless + Static** |
|---|---|---|---|---|
| **First Contentful Paint (FCP)** | ⚡ **Exceptional** (Zero JS by default) | 🟢 Fast (Server rendered) | 🟡 Moderate (Client bundle dependent) | 🟢 Fast |
| **SEO & OpenGraph Metadata** | 🟢 Native static HTML generation | 🟢 Native metadata API | 🔴 Requires pre-rendering / SSR | 🟢 Native |
| **Editorial Storytelling & Motion** | 🟢 Perfect for interactive islands (Swiper, GSAP, Maps) | 🟢 Excellent support | 🟢 Excellent | 🟡 Theme-constrained |
| **Hosting Complexity & Cost** | 🟢 Zero-server hosting (Vercel, Cloudflare, Netlify) | 🟡 Node.js runtime / serverless required | 🟢 Static hosting | 🔴 PHP/MySQL server required |
| **Suitability for Luxury Hotel** | ⭐ **Top Recommendation (Grade: A+)** | ⭐ **Strong Alternative (Grade: A)** | ⚠️ Not ideal for SEO (Grade: B-) | ⚠️ Legacy maintenance overhead |

### Recommended Stack: **Astro + Tailwind CSS + Swiper / GSAP**
1. **Why Astro?** Luxury hotel websites are 85% content-driven (rooms, photography, dining menus, conference specs) and 15% interactive (booking widget, room carousels, full-screen menu, date pickers). Astro renders pure static HTML by default, achieving near-perfect 100/100 Core Web Vitals, while hydrating interactive components ("islands") only when scrolled into view.
2. **Alternative:** If dynamic server-side availability checking or member authentication is planned for Phase 2, **Next.js (React)** is the most versatile enterprise choice.

---

## 2. Integration Architecture: Booking Engine & Conversions

Grand Pasundan operates an Internet Booking Engine (IBE) on subdomain:
`https://booking.grandpasundanhotel.com/` (Hotel Code: `GPSH`).

### Recommended URL Dispatch Schema
Rather than embedding an iframe (which damages mobile UX, causes scroll jank, and prevents cross-domain analytics tracking), implement **deep-linking with URL parameter pre-population**:

```text
Target URL:
https://booking.grandpasundanhotel.com//offers_promo
  ?hotel=GPSH
  &room_id={ROOM_ID}
  &checkin={YYYY-MM-DD}
  &checkout={YYYY-MM-DD}
  &adults={N}
  &children={N}
  &utm_source=grandpasundan_web
  &utm_medium=cta_button
  &utm_campaign=redesign_2026
```

### High-Touch Conversions: WhatsApp Business API Routing
For MICE (Malibu Dome & 13 meeting rooms) and Wedding bookings, direct booking forms often have high bounce rates. Implement a direct click-to-WhatsApp link pre-filled with structured text:

```text
https://wa.me/628112266959?text=Hello%20Grand%20Pasundan,%20I%20would%20like%20to%20inquire%20about%20booking%20the%20Malibu%20Dome%20for%20an%20event%20on%20[Date].
```

---

## 3. Core Web Vitals & Performance Engineering Targets

| Metric | Target Threshold | Implementation Strategy |
|---|---|---|
| **Largest Contentful Paint (LCP)** | `< 1.8s` | Preload hero background image; serve modern WebP/AVIF with responsive `srcset`; self-host font files with `font-display: swap`. |
| **Cumulative Layout Shift (CLS)** | `< 0.02` | Explicit `width` and `height` on all image containers and hero slides; reserve layout space for sticky headers. |
| **Interaction to Next Paint (INP)** | `< 100ms` | Minimal main-thread JS execution; passive event listeners on scroll and carousel touch events. |
| **Accessibility (a11y)** | `WCAG 2.1 AA (Score: 100)` | Semantic HTML (`<header>`, `<main>`, `<nav>`, `<article>`), high contrast ratio (`> 4.5:1`), explicit `aria-label` on all icon and burger triggers. |

---

## 4. Design & Development Roadmap

```
PHASE 1: Foundations & Content Architecture [CURRENT PHASE]
  ├── Design Audit & Benchmark Analysis (Schlosshotel Kitzbühel) [COMPLETED]
  ├── Navigation Tree & Dual-Action Flows [COMPLETED]
  ├── Design Tokens (Colors, Typography, Spacing, Elevation) [COMPLETED]
  └── Component Specifications & Responsive Rules [COMPLETED]

PHASE 2: Project Setup & Framework Selection (Next Step)
  ├── User approval on framework (Astro vs Next.js vs Vite)
  ├── Initialize clean project repository with chosen toolchain
  ├── Configure Tailwind CSS or CSS Modules with Design Tokens (:root)
  └── Setup global layouts (Header, Fullscreen Menu, Footer)

PHASE 3: Component Library Development
  ├── Build atomic components: UI-Btn, UI-Badge, UI-Card
  ├── Implement Hero Carousel with floating promo badge
  ├── Implement MOD-TextPicV2 (Editorial storytelling)
  ├── Implement MOD-RoomsTeaser (Swiper room carousel)
  └── Implement MOD-ServiceSlider (A Day in the Life timeline)

PHASE 4: Content Migration & Page Templates
  ├── Populate 14 Room pages from grandpasundanhotel.com
  ├── Populate 7 Facility pages (Pool, SPA, Karaoke, Coffee Shop, Library)
  ├── Populate Meetings & Convention page (Capacity chart, Jayagiri/Malibu tabs)
  └── Populate Weddings page (Silver/Gold/Platinum packages)

PHASE 5: Integration, Analytics & Launch
  ├── Deep link validation with booking.grandpasundanhotel.com
  ├── WhatsApp direct conversion routing verification
  ├── Cross-browser testing (Desktop, iPad, iPhone, Android)
  └── Lighthouse 95+ performance optimization & launch
```
