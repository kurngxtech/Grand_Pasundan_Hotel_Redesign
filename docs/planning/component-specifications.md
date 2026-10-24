# Component Specifications: Reusable UI Building Blocks

> **Document Version:** 1.0.0  
> **Target Project:** Grand Pasundan Convention Hotel Redesign  
> **Source Model:** Reverse-engineered modular components (`DNA-module`) from Schlosshotel Kitzbühel  
> **Target Audience:** Frontend Architects, UI Engineers, and AI Coding Agents

---

## 1. Component Architecture Overview

All UI blocks are modeled as self-contained, composable modules following standard naming conventions:
- `SECT-*`: Top-level structural blocks (Header, Hero, Footer).
- `MOD-*`: Decoupled content modules placed inside the main content container.
- `UI-*`: Atomic and molecular reusable elements (Buttons, Badges, Cards, Carousels).

---

## 2. Global Navigation: Header & Fullscreen Drawer

### 2.1 Component: `SECT-Header`
Persistent, responsive navigation bar supporting transparent-to-solid transitions upon page scroll.

#### DOM Structure Specification
```html
<header class="sect-header" data-state="transparent">
  <!-- Micro Top Bar (Desktop only) -->
  <div class="sect-header__micro-bar">
    <a href="/vouchers" class="micro-link">Vouchers</a>
    <a href="tel:+62226043135" class="micro-link">Tel. 022 6043135</a>
    <div class="lang-selector">
      <button aria-label="Select Language">EN / ID</button>
    </div>
  </div>

  <!-- Main Navigation Row -->
  <div class="sect-header__main-row">
    <!-- Brand Identity -->
    <a href="/" class="sect-header__logo" aria-label="Grand Pasundan Convention Hotel">
      <img src="/logo-pasundan.svg" alt="Grand Pasundan Hotel" width="160" height="48" />
    </a>

    <!-- Desktop Center Quick Links -->
    <nav class="sect-header__nav-quick" aria-label="Primary Navigation">
      <a href="/rooms">Rooms & Suites</a>
      <a href="/meetings">Meetings & MICE</a>
      <a href="/weddings">Weddings</a>
      <a href="/facilities">Facilities</a>
      <a href="/offers">Offers</a>
    </nav>

    <!-- Right Action Cluster -->
    <div class="sect-header__actions">
      <!-- Dual Action Buttons -->
      <a href="https://booking.grandpasundanhotel.com/?hotel=GPSH" class="ui-btn ui-btn--primary">BOOK</a>
      <a href="/contact" class="ui-btn ui-btn--outline">ENQUIRE</a>
      
      <!-- Hamburger Toggle Trigger -->
      <button class="sect-header__burger" aria-label="Open Navigation Menu" aria-expanded="false">
        <span class="burger-line"></span>
        <span class="burger-line"></span>
      </button>
    </div>
  </div>
</header>
```

#### Behavioral Rules
- **Scroll Behavior:** At `scrollY < 50px`, the header is transparent. At `scrollY >= 50px`, it transitions smoothly to `background-color: var(--gph-color-canvas-primary)` with a subtle bottom border (`var(--gph-color-border-subtle)`).
- **Z-Index:** `z-index: 1000` to sit permanently above page content and carousels.

---

### 2.2 Component: `SECT-FullscreenMenu`
An immersive fullscreen overlay that eliminates cluttered multi-level dropdowns.

#### Layout Schema
- **Left Column (Primary Pillars):** Large display serif links (`Playfair Display`, ~`2.5rem`), with staggered entrance animations.
  1. *The Hotel* (`/about`)
  2. *Rooms & Suites* (`/rooms`)
  3. *Meetings & Conventions* (`/meetings`)
  4. *Weddings & Celebrations* (`/weddings`)
  5. *Dining & Cafeteria* (`/facility/coffee-shop`)
  6. *Wellness & Leisure* (`/facility/pool`)
  7. *Special Offers* (`/offers`)
  8. *Journal & News* (`/news`)
- **Right Column (Contextual Content & Contact):**
  - Instant contact card (WhatsApp, Telephone, Address).
  - Dynamic preview image that updates on hover over left-hand navigation items.

---

## 3. Core Action: Dual CTA System (`UI-DualCTA`)

### Concept
Luxury hospitality requires addressing two mindsets simultaneously: the transactional guest who wants immediate price and booking, and the high-touch guest (weddings, MICE, VIP suites) who requires bespoke inquiry.

```
+-------------------------------------------------------------+
|   [  BOOK  ]                 [  ENQUIRE  ]                  |
|   Background: Pure White     Background: Transparent/Dark   |
|   Text: Dark Charcoal        Text: Pure White               |
|   Border: None               Border: 1px Solid White / Gold |
|   Action: External IBE       Action: Lead Modal / WhatsApp  |
+-------------------------------------------------------------+
```

#### CSS Implementation
```css
.ui-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-family: var(--gph-font-sans);
  font-size: var(--gph-text-btn);
  font-weight: var(--gph-font-weight-btn);
  letter-spacing: var(--gph-tracking-btn);
  text-transform: uppercase;
  text-decoration: none;
  padding: 12px 24px;
  border-radius: var(--gph-radius-sharp);
  transition: all var(--gph-transition-smooth);
  cursor: pointer;
}

/* Primary Button (Instant Booking) */
.ui-btn--primary {
  background-color: var(--gph-color-text-primary);
  color: var(--gph-color-canvas-primary);
  border: 1px solid var(--gph-color-text-primary);
}
.ui-btn--primary:hover {
  background-color: var(--gph-color-gold-500);
  border-color: var(--gph-color-gold-500);
  color: #FFFFFF;
}

/* Secondary Button (Structured Inquiry / Consultation) */
.ui-btn--outline {
  background-color: transparent;
  color: var(--gph-color-text-primary);
  border: 1px solid rgba(255, 255, 255, 0.4);
}
.ui-btn--outline:hover {
  border-color: var(--gph-color-text-primary);
  background-color: rgba(255, 255, 255, 0.08);
}
```

---

## 4. Content Storytelling Modules

### 4.1 Component: `MOD-TextPicV2` (Editorial Intro)
Asymmetric layout combining editorial typography with architectural photography.

#### Layout Grid
- **Desktop (`>= 1025px`):** 12-column grid.
  - Image: Columns 1–6 (Full-height framed photo with subtle aspect ratio `4:5`).
  - Text Content: Columns 7–12 (Padded, vertically centered).
    - Eyebrow: `var(--gph-text-eyebrow)`, uppercase, tracked (`2.2px`), gold or muted white.
    - Title: `var(--gph-text-h2)`, serif font, high-contrast white.
    - Body: 2–3 paragraphs of evocative brand narrative.
    - CTA: Discover More link with forward arrow (`→`).
- **Mobile (`<= 767px`):** Single-column stacked (Image top, text below).

---

### 4.2 Component: `MOD-RoomsTeaser` (Accommodations Carousel)
Swiper/Slick-powered responsive slider showcasing luxury rooms with specs, tonight's pricing, and dual actions.

#### Card Anatomy
1. **Media Frame (`16:10` or `4:3`):** High-resolution room photography with zoom-on-hover effect.
2. **Category / Name:** Serif title (`Classic Deluxe Twin`, `Pasundan 3-Bedroom Suite`).
3. **Spec Badges:** Capacity (e.g., `2 Guests`), Dimensions (`32 m²`), Bed Configuration (`1 King / 2 Twins`).
4. **Rate Footnote:** *"From IDR 608,000 / night incl. breakfast"*.
5. **Action Bar:** Compact dual CTAs (`BOOK` + `ENQUIRE`) + `Details →` link.

---

### 4.3 Component: `MOD-ServiceSlider` ("A Day in the Life" Timeline)
A signature storytelling component that guides visitors through a chronological 24-hour stay.

#### Data Schema & State Representation
```typescript
interface TimelineItem {
  id: string;
  timeLabel: string;        // e.g. "06:30 AM", "10:30 AM"
  category: string;         // e.g. "Morning Serenity", "Plenary Excellence"
  headline: string;         // e.g. "Pustaka Pasundan: Cultural Morning Reading"
  description: string;      // 50-80 word narrative
  imageSrc: string;         // WebP optimized photography
  imageAlt: string;
  linkUrl: string;          // Direct link to corresponding facility page
}
```

#### Layout & Interaction
- **Sticky Time Indicator:** A prominent serif time badge (`var(--gph-text-display)`) that remains sticky or smoothly advances as the user scrolls.
- **Scroll Sync:** As each time block enters the viewport, the background imagery or active card shifts smoothly using CSS scroll-driven animations or IntersectionObserver.

---

### 4.4 Component: `MOD-Quotes` (Executive / GM Hospitality Quote)
Humanizes the hotel brand by spotlighting leadership and culinary / cultural dedication.

#### Specifications
- **Layout:** Full-width centered container or two-column split with portrait photography.
- **Typography:** Large italic serif quote marks, followed by clean editorial text (~`1.25rem`).
- **Signature & Title:** General Manager name, followed by title in tracked uppercase sans-serif (*"GENERAL MANAGER — GRAND PASUNDAN CONVENTION HOTEL"*).

---

### 4.5 Component: `MOD-Siteplan` (Interactive Facility & Property Map)
Crucial for a complex property like Grand Pasundan, which houses 155 rooms, 7 suites, 13 meeting rooms, the Malibu Dome, multiple dining outlets, and leisure pools across two distinct wings (Main Building & West Wing).

#### Technical Specification
- **Base Layer:** High-detail illustrated or aerial 3D rendering of the hotel complex.
- **Interactive Hotspot Pins:** Clickable SVG circular pulse pins placed at coordinate percentages (`left: 45%; top: 32%`).
- **Popup Card / Drawer:** Clicking a pin reveals a preview card (Title, photo, operating hours, and a button to *"Explore Facility"*).
- **Filter Bar:** Category filters above the map: `[All]`, `[Rooms & Suites]`, `[Meetings & MICE]`, `[Dining]`, `[Recreation]`.

---

## 5. Global Footer (`SECT-Footer`)

### Layout Grid (4 Columns on Desktop, 1 Column on Mobile)
1. **Column 1 (Identity & Contact):** Hotel crest, physical address (Jl. Peta No. 147–149 Bandung), direct telephone, and reservation email.
2. **Column 2 (Navigation):** Rooms, MICE, Weddings, Facilities, Special Offers.
3. **Column 3 (Stay Connected & Direct WhatsApp):** Direct WhatsApp concierge link, Instagram feed link, Facebook link.
4. **Column 4 (Newsletter & Accreditations):** VIP Newsletter subscription form with data privacy consent checkbox, followed by 4-star hospitality credentials.
5. **Sub-Footer Row:** Copyright statement, privacy policy, terms & conditions, and accessibility declaration.
