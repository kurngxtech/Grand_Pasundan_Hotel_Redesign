# Responsive Design Rules & Viewport Architecture

> **Document Version:** 1.0.0  
> **Target Project:** Grand Pasundan Convention Hotel Redesign  
> **Source Model:** Responsive engineering analyzed on Schlosshotel Kitzbühel  
> **Target Audience:** Frontend Developers, Layout Engineers, and AI Coding Agents

---

## 1. Breakpoint Definitions

The responsive system utilizes 4 primary breakpoints with standard minimum/maximum thresholds:

| Breakpoint Tier | Media Query Range | Target Devices | Layout Behavior |
|---|---|---|---|
| **Mobile Standard (`sm`)** | `max-width: 767px` | Smartphones (iPhone, Android: 375px–430px) | Single-column linear stack, bottom action bar, horizontal touch swipers |
| **Tablet (`md`)** | `min-width: 768px` and `max-width: 1024px` | iPads, Android tablets (768px–1024px) | 2-column hybrid grids, condensed header, overlay navigation |
| **Desktop Standard (`lg`)** | `min-width: 1025px` and `max-width: 1440px` | Laptops, MacBooks, standard monitors | Full 12-column grid, horizontal quick links + dual CTAs |
| **Widescreen (`xl`)** | `min-width: 1441px` | 2K, 4K, Ultra-wide displays | Container max-width clamped at `1440px` with generous side margins |

### Standard Media Query Variables (CSS)
```css
@custom-media --viewport-mobile (max-width: 767px);
@custom-media --viewport-tablet (min-width: 768px) and (max-width: 1024px);
@custom-media --viewport-desktop (min-width: 1025px);
@custom-media --viewport-widescreen (min-width: 1441px);
```

---

## 2. Fluid Typography Math & Scaling

Rather than jumping abruptly between fixed pixel sizes at breakpoints, the system implements continuous fluid scaling via CSS `clamp()`:

### Mathematical Formula
$$\text{Fluid Size} = \text{clamp}\left(\text{Min Size},\ \text{Base} + \left(\frac{\text{Max} - \text{Min}}{\text{Max VP} - \text{Min VP}}\right) \times 100\text{vw},\ \text{Max Size}\right)$$

Assuming a baseline from `375px` (Mobile) to `1440px` (Desktop):

| Type Role | Mobile Size | Desktop Size | CSS Implementation Formula |
|---|---|---|---|
| **Display Title** | `36px` (`2.25rem`) | `72px` (`4.5rem`) | `clamp(2.25rem, 1.45rem + 3.38vw, 4.5rem)` |
| **H1 (Page Header)** | `28px` (`1.75rem`) | `48px` (`3.0rem`) | `clamp(1.75rem, 1.3rem + 1.88vw, 3.0rem)` |
| **H2 (Section Editorial)** | `24px` (`1.5rem`) | `38px` (`2.375rem`) | `clamp(1.5rem, 1.19rem + 1.31vw, 2.375rem)` |
| **H3 (Card Heading)** | `20px` (`1.25rem`) | `26px` (`1.625rem`) | `clamp(1.25rem, 1.11rem + 0.56vw, 1.625rem)` |
| **Body Paragraph** | `15px` (`0.9375rem`) | `17px` (`1.0625rem`) | `clamp(0.9375rem, 0.89rem + 0.19vw, 1.0625rem)` |
| **Eyebrow / Tracked Tag**| `11px` (`0.6875rem`) | `13px` (`0.8125rem`) | `clamp(0.6875rem, 0.64rem + 0.19vw, 0.8125rem)` |

---

## 3. Structural Responsive Reflow Patterns

### 3.1 Header Transformation
- **Desktop (`>= 1025px`):**
  - Micro-bar visible at top (Phone, Language, Vouchers).
  - Center quick links visible (`Rooms`, `Meetings`, `Weddings`, `Facilities`, `Offers`).
  - Right action cluster shows both `BOOK` and `ENQUIRE` + hamburger icon.
- **Tablet (`768px – 1024px`):**
  - Micro-bar hidden to conserve vertical screen estate.
  - Quick links collapse into the hamburger drawer.
  - `BOOK` button remains visible alongside the hamburger trigger.
- **Mobile (`<= 767px`):**
  - Ultra-compact header height: `64px`.
  - Brand logo aligned left.
  - Right side displays only compact `BOOK` pill button and the hamburger trigger.
  - Persistent bottom sticky action bar appears (WhatsApp chat button + Quick Booking trigger).

---

### 3.2 Content Grid Reflows

```
DESKTOP (12 Columns)                TABLET (8 Columns)              MOBILE (4 Columns)
+-----------------------+           +-------------------+           +---------------+
| Img (6)  |  Text (6)  |           | Img (4) | Text (4)|           |    Img (4)    |
|          |            |    -->    |         |         |    -->    +---------------+
|          |            |           |         |         |           |    Text (4)   |
+-----------------------+           +-------------------+           +---------------+
```

- **Rooms & Facilities Catalogs (`MOD-RoomsTeaser`):**
  - **Desktop:** Multi-column carousel showing 3–4 room cards simultaneously with navigation arrows.
  - **Tablet:** 2 room cards visible with drag/touch swipe gesture enabled.
  - **Mobile:** 1.15 cards visible (the trailing card peek provides visual affordance that horizontal scrolling is available) with native CSS scroll-snap (`scroll-snap-type: x mandatory`).

- **Asymmetric Editorial Layouts (`MOD-TextPicV2`):**
  - **Desktop:** 50/50 split with overlapping depth layers and offset text container.
  - **Mobile:** Converts to a top-to-bottom vertical column: Image first (full-bleed or 16px inset), followed by eyebrow, heading, narrative body, and button.

---

### 3.3 "A Day in the Life" Timeline (`MOD-ServiceSlider`)
- **Desktop:** Split view with fixed/sticky time numeral on the left (`06:30 AM`), and corresponding photo + experience card on the right.
- **Mobile:** Converts to a linear vertical timeline:
  - Time badge appears as an inline pill (`06:30 AM — MORNING SERENITY`).
  - Full-width experience photo (`16:9` ratio).
  - Text description below photo with direct facility navigation link.

---

## 4. Image Aspect Ratios & Responsive Delivery

To prevent Cumulative Layout Shift (CLS) and ensure ultra-crisp display across Retina/HiDPI screens:

| Component | Desktop Ratio | Mobile Ratio | Art Direction Notes |
|---|---|---|---|
| **Hero Carousel** | `21:9` or `16:9` | `4:5` (Vertical portrait) | Focus on room intimacy or architectural facade rather than wide panorama |
| **Room Card** | `16:10` | `16:10` | Keep consistent to avoid card height jumping during swipe |
| **Editorial Picture** | `4:5` | `1:1` or `4:3` | Cropped to focus on materials, textures, and details |
| **Service Timeline** | `16:9` | `16:9` | High-energy lifestyle and dining photography |

### HTML Picture Delivery Pattern for Agents
```html
<picture>
  <!-- Mobile Portrait Crop (WebP) -->
  <source media="(max-width: 767px)" srcset="/img/hero-portrait.webp 800w" type="image/webp" />
  <!-- Desktop Wide Crop (WebP) -->
  <source media="(min-width: 768px)" srcset="/img/hero-wide.webp 1920w" type="image/webp" />
  <!-- Fallback JPG -->
  <img src="/img/hero-wide.jpg" alt="Grand Pasundan Convention Hotel" loading="eager" width="1920" height="1080" />
</picture>
```
