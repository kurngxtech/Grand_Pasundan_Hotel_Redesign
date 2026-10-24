# Design Tokens: Luxury Hospitality System

> **Document Version:** 1.0.0  
> **Target Project:** Grand Pasundan Convention Hotel  
> **Source Baseline:** Reverse-engineered computed styles from Schlosshotel Kitzbühel  
> **Specification Standard:** W3C Design Tokens Community Group format & CSS Custom Properties (`:root`)

---

## 1. Architectural Strategy

This design token system translates the European luxury benchmark into a production-ready token foundation tailored for **Grand Pasundan Convention Hotel**. It supports:
1. **Light / Dark Mode Agnostic Luxury:** High-contrast dark luxury base with warm accents.
2. **Editorial Typography Pairing:** High-contrast Serif headings paired with tracked uppercase Sans-Serif metadata.
3. **Fluid Scale:** Proportional fluid typography and spacing that adapts smoothly from mobile (`375px`) to large displays (`1920px+`).

---

## 2. Color System

### 2.1 Reference Palette (Schlosshotel Kitzbühel)

| Token Name | Hex Code | RGB | Purpose |
|---|---|---|---|
| `--color-ref-canvas` | `#300316` | `rgb(48, 3, 22)` | Deep Velvet Burgundy (Primary Background) |
| `--color-ref-surface` | `#42041E` | `rgb(66, 4, 30)` | Dark Wine / Secondary Canvas |
| `--color-ref-text-high` | `#FFFFFF` | `rgb(255, 255, 255)` | Pure White Primary Text |
| `--color-ref-text-muted` | `#FFFFFF80` | `rgba(255, 255, 255, 0.5)` | 50% Muted White Subtitles & Captions |
| `--color-ref-border` | `#FFFFFF33` | `rgba(255, 255, 255, 0.2)` | Subtle Ghost Borders |
| `--color-ref-gold-warm` | `#C49A5A` | `rgb(196, 154, 90)` | Muted Heritage Gold (Accents & Highlights) |

### 2.2 Adapted Palette for Grand Pasundan Convention Hotel
*Combining Sundanese royal heritage ("Preserve Pasundan Tradition") with contemporary 4-star luxury hospitality.*

```css
:root {
  /* ==========================================================================
     BRAND PALETTE (PASUNDAN HERITAGE & WARM LUXURY)
     ========================================================================== */
  
  /* Primary Royal Backgrounds */
  --gph-color-canvas-primary: #1C1917;    /* Deep Warm Obsidian / Stone 900 */
  --gph-color-canvas-surface: #292524;    /* Warm Teak Charcoal / Stone 800 */
  --gph-color-canvas-elevated: #44403C;   /* Warm Slate / Stone 700 */

  /* Pasundan Heritage Accents */
  --gph-color-gold-500: #D4AF37;          /* Traditional West Javanese Gold */
  --gph-color-gold-400: #E5C358;          /* Light Shimmer Gold (Hover) */
  --gph-color-gold-600: #B89628;          /* Deep Antique Bronze */
  --gph-color-terracotta: #A85A3C;        /* Natural Clay / Roof Tile Accent */
  
  /* Text & Readability */
  --gph-color-text-primary: #FAFAF9;      /* Warm Off-White (Stone 50) */
  --gph-color-text-secondary: #D6D3D1;    /* Muted Linen (Stone 300) */
  --gph-color-text-muted: #A8A29E;        /* Subtle Granite (Stone 400) */
  --gph-color-text-inverse: #1C1917;      /* Dark Text for Gold/White buttons */

  /* UI Borders & Dividers */
  --gph-color-border-subtle: rgba(250, 250, 249, 0.12);
  --gph-color-border-strong: rgba(212, 175, 55, 0.35);

  /* Status & Accents */
  --gph-color-success: #2E7D32;
  --gph-color-alert: #C62828;
  --gph-color-whatsapp: #25D366;          /* Official Direct Conversion Color */
}
```

---

## 3. Typography System

### 3.1 Font Families
- **Editorial Serif:** `Playfair Display`, `Cormorant Garamond`, or `Cinzel` (Fallback: `Georgia`, `serif`).
- **Body & Structural Sans:** `Plus Jakarta Sans`, `Assistant`, or `Inter` (Fallback: `system-ui`, `-apple-system`, `sans-serif`).

### 3.2 Type Scale (Fluid Clamp Implementation)

```css
:root {
  /* Font Family Stacks */
  --gph-font-serif: 'Playfair Display', Georgia, serif;
  --gph-font-sans: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;

  /* Fluid Typography Scale: clamp(min, preferred-calc, max) */
  
  /* Display / Hero Titles */
  --gph-text-display: clamp(2.5rem, 1.8rem + 3.5vw, 5.25rem);
  --gph-leading-display: 1.05;
  --gph-tracking-display: -0.02em;

  /* Heading 1 (Page Title) */
  --gph-text-h1: clamp(2.0rem, 1.5rem + 2.5vw, 3.75rem);
  --gph-leading-h1: 1.15;
  --gph-tracking-h1: -0.01em;

  /* Heading 2 (Section Title - Serif Editorial) */
  --gph-text-h2: clamp(1.6rem, 1.25rem + 1.8vw, 2.75rem);
  --gph-leading-h2: 1.25;

  /* Heading 3 (Card & Feature Title) */
  --gph-text-h3: clamp(1.25rem, 1.1rem + 0.8vw, 1.75rem);
  --gph-leading-h3: 1.35;

  /* Eyebrow / Tagline (All Caps, Tracked) */
  --gph-text-eyebrow: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
  --gph-tracking-eyebrow: 0.18em;  /* Benchmark: 2.2px tracking */
  --gph-leading-eyebrow: 1.4;

  /* Body Copy */
  --gph-text-body-lg: clamp(1.125rem, 1.05rem + 0.35vw, 1.375rem);
  --gph-leading-body-lg: 1.65;

  --gph-text-body: clamp(0.9375rem, 0.9rem + 0.2vw, 1.0625rem);
  --gph-leading-body: 1.6;

  --gph-text-caption: clamp(0.75rem, 0.72rem + 0.15vw, 0.875rem);
  --gph-leading-caption: 1.4;

  /* Button Labels */
  --gph-text-btn: 0.8125rem;
  --gph-tracking-btn: 0.16em;      /* Benchmark: 1.6px uppercase tracking */
  --gph-font-weight-btn: 600;
}
```

---

## 4. Spacing, Layout & Grid Scale

The layout adheres to a modular 8-point base grid system combined with fluid page padding.

```css
:root {
  /* Base Spacing Scale */
  --gph-space-2xs: 0.25rem;   /* 4px */
  --gph-space-xs:  0.5rem;    /* 8px */
  --gph-space-sm:  0.75rem;   /* 12px */
  --gph-space-md:  1.0rem;    /* 16px */
  --gph-space-lg:  1.5rem;    /* 24px */
  --gph-space-xl:  2.0rem;    /* 32px */
  --gph-space-2xl: 3.0rem;    /* 48px */
  --gph-space-3xl: 4.5rem;    /* 72px */
  --gph-space-4xl: 6.0rem;    /* 96px */
  --gph-space-5xl: 8.0rem;    /* 128px */

  /* Page Padding & Container Bounds */
  --gph-container-max: 1440px;
  --gph-container-narrow: 1040px;
  --gph-gutter-fluid: clamp(1.25rem, 0.75rem + 2.5vw, 5rem);
  --gph-section-gap: clamp(4rem, 2.5rem + 7.5vw, 10rem);
}
```

---

## 5. Component Tokens (Buttons, Cards & Borders)

```css
:root {
  /* Corner Radii */
  /* Benchmark observation: 0px creates sharp architectural sophistication */
  --gph-radius-sharp: 0px;
  --gph-radius-sm: 2px;
  --gph-radius-md: 4px;
  --gph-radius-pill: 9999px; /* For floating badges and indicator pills */

  /* Elevation Shadows */
  --gph-shadow-subtle: 0 4px 20px rgba(0, 0, 0, 0.25);
  --gph-shadow-floating: 0 12px 40px rgba(0, 0, 0, 0.45);
  --gph-shadow-gold: 0 8px 30px rgba(212, 175, 55, 0.25);

  /* Micro-Interaction Transitions */
  --gph-transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --gph-transition-smooth: 350ms cubic-bezier(0.25, 1, 0.5, 1);
  --gph-transition-cinematic: 600ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

---

## 6. How AI Coding Agents Should Consume These Tokens

When building components with Tailwind CSS, Vanilla CSS, or CSS Modules, agents must map directly to these variables:

```css
/* Example Component Implementation */
.cta-button-book {
  background-color: var(--gph-color-text-primary);
  color: var(--gph-color-canvas-primary);
  font-family: var(--gph-font-sans);
  font-size: var(--gph-text-btn);
  font-weight: var(--gph-font-weight-btn);
  letter-spacing: var(--gph-tracking-btn);
  text-transform: uppercase;
  padding: 14px 28px;
  border: none;
  border-radius: var(--gph-radius-sharp);
  transition: all var(--gph-transition-smooth);
}

.cta-button-book:hover {
  background-color: var(--gph-color-gold-500);
  color: #FFFFFF;
  box-shadow: var(--gph-shadow-gold);
}
```
