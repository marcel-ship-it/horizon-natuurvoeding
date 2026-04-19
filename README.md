# Handoff: Horizon Natuurvoeding Website

## Overview
This is a complete website redesign for **Horizon Natuurvoeding**, a Dutch organic food company (46 jaar biologisch pionier). The design implements the official Horizon brandbook: Clarendon typography, horizon-gradient identity, and a hero section with the signature horizon line cutting through the headline text.

## About the Design Files
The files in this bundle are **design references created in HTML** — high-fidelity prototypes showing the intended look and behavior. They are NOT production code to ship directly. The task is to **recreate these HTML designs in your codebase** (or, if starting fresh, choose the most appropriate framework and implement them there), using established patterns and libraries.

The main reference file is: `Horizon Natuurvoeding - Website.html`

## Fidelity
**High-fidelity.** Colors, typography, spacing, interactions, and animations are all final and pixel-precise. The developer should recreate the UI pixel-perfectly using the codebase's patterns and design system.

---

## Screens / Views

### 1. Navigation Bar (sticky)
- **Purpose**: Primary navigation, logo, CTA
- **Layout**: Fixed top, full width, height ~64px, flex row, space-between
- **Background**: `#fff0d8` (cream) with bottom border `1px solid rgba(0,156,220,0.15)`
- **Logo**: HORIZON SVG wordmark (left), below it `NATUURVOEDING` in DM Sans Medium 8px, letter-spacing 0.33em, uppercase, color `#009cdc`
- **Nav links**: DM Sans 500, 14px, color `#009cdc`, uppercase, letter-spacing 0.05em
- **CTA button**: "PRAAT MET ONS →", background `#60d553` (green), white text, border-radius 99px, padding 10px 20px
- **Behavior**: Hides on scroll down, reappears on scroll up

---

### 2. Hero Section
- **Purpose**: Brand statement, primary CTA
- **Structure**: Two zones stacked vertically:

#### 2a. Hero Sky (blue gradient)
- Background: `linear-gradient(to bottom, #009cdc 0%, #b8e8f5 50%, #fff0d8 100%)`
- Full viewport width, height ~62vh
- Canvas overlay: animated sun/moon that moves across the sky based on real time (SunCalc library). Sun is a large soft radial gradient glow. At night, a moon glow appears.
- Large headline: "SAMEN / BEREIKEN / WE 'M WEL." in **Clarendon Extra Bold 800**, `clamp(3.4rem, 10.5vw, 9.5rem)`, color `#fff0d8` (cream), uppercase, text-align center, line-height 1

#### 2b. Hero Earth (cream zone)
- Background: `#fff0d8`
- **Horizon line effect**: The cream earth zone overlaps the bottom of "WE 'M WEL." via negative margin-top. The JavaScript function `adjustHorizonCut()` dynamically calculates the exact overlap so the horizon line cuts through at 76% of the headline height (letters appear to "stand on the horizon", bottom ~24% disappears into the earth). This recalculates on every resize.
- **Mission statement**: "100% BIOLOGISCH / VOOR 100% VAN IEDEREEN" in Clarendon 800, `clamp(1.4rem, 3.5vw, 3rem)`, color `#009cdc`, centered. Position: dynamically centered between the horizon cut and the CTA buttons (via JS calculation).
- **CTAs**: 
  - "Laten we groeien →" — green pill button, `#60d553`
  - "Zien wat we maken →" — ghost/text link, `#009cdc`
- **Centering JS**: After setting horizon cut, JS finds `.hero-cta` and `.hero-missie`, calculates available space, and sets `missieEl.style.marginTop` so spacing above = spacing below.

---

### 3. Stats Section
- **Background**: `#009cdc` (blue)
- **Layout**: 3-column grid, centered
- **Numbers**: Clarendon 800, `clamp(2.2rem, 4.5vw, 4rem)`, color `#fff0d8`, white-space nowrap
- **Labels**: DM Sans 500, 12px, letter-spacing 0.13em, uppercase, color `#fff0d8` with 70% opacity
- **Content**: "46 JAAR / Biologisch pionier", "4 MERKEN / Eén missie", "30+ / Gepassioneerde mensen"

---

### 4. Quote Section (Over ons)
- **Background**: `#fff0d8`
- **Layout**: 2-col grid (quote left, body text right)
- **Quote**: "Geen water bij de wijn." — Clarendon 800, large, color `#009cdc`
- **Body**: DM Sans 400, 16px, color `#000`, line-height 1.6

---

### 5. Products Section
- **Background**: `#fff0d8`
- **Layout**: Asymmetric CSS grid, product cards with images and labels
- **Labels**: DM Sans 500, 12px, letter-spacing 0.13em, uppercase, `#009cdc`, border-radius 99px, background white
- **Products**: Horizon notenpasta, Pindakaas, Amandelpasta, Cashewpasta, Tahin

---

### 6. Values Section
- **Sky zone**: Blue gradient background, headline "Vijf waarden. Eén koers." in Clarendon
- **Values grid**: 5 circle icons + descriptions. Circles alternate blue/green (`#60d553`)
- **Circle**: 120px diameter, centered label in Clarendon 800 14px

---

### 7. Brands Section
- **Background**: `#009cdc` (blue)
- **Layout**: 4-column card grid
- **Cards**: White background, product image, brand name (Clarendon), description (DM Sans), "Zien wat we maken →" link
- **Brands**: Horizon, Monki, Jori, Krekeltje

---

### 8. B2B Section
- **Background**: `#fff0d8`
- **Layout**: 3-col text grid + CTA button
- **CTA**: "Praat met ons →", green pill button

---

### 9. Footer
- **Background**: `#009cdc`
- **Layout**: 3-col grid (1.2fr 1fr 1fr)
- **Logo col**: HORIZON SVG (height 28px, color `#fff0d8`) + tagline "SAMEN BEREIKEN WE 'M WEL." in DM Sans 500, 8px, letter-spacing 0.33em, uppercase, white-space nowrap + EU Bio badge image
- **Other cols**: Bezoek, Contact info in DM Sans, cream color
- **Bottom bar**: `#003a5c`, copyright info left + SKAL right

---

## Interactions & Behavior

### Scroll animations
- All major sections have `.fade-on-scroll` class — `opacity: 0` by default, `transform: translateY(20px)`
- IntersectionObserver triggers `.visible` class: `opacity: 1, transform: none`, transition 600ms ease-out

### Navigation hide/show
- Nav hides (`transform: translateY(-110%)`) when scrolling down past 80px
- Reappears on scroll up

### Sun/Moon animation
- Uses SunCalc CDN library for sunrise/sunset times (Utrecht, NL: 52.1°N, 5.1°E)
- Canvas element overlaid on hero sky
- Daytime: large radial glow moves left→right following sun arc
- Nighttime: smaller moon glow moves right→left
- On page load: sweeps from start of day/night to current position over 3 seconds
- Dev tool: press **Shift+T** to show time scrubber panel

### Horizon cut (JS-driven)
```js
function adjustHorizonCut() {
  const lastLine = document.querySelector('.hl-wemwel');
  const heroEarth = document.querySelector('.hero-earth');
  const targetTop = lineRect.top + lineRect.height * 0.76; // 24% cut
  // ... sets margin-top and padding-top on heroEarth
  // Then centers .hero-missie between horizon and .hero-cta
}
```
Fires on: DOMContentLoaded (after 300ms), after fonts load (1300ms), on window resize.

---

## Design Tokens

### Colors
| Token | Value | Usage |
|-------|-------|-------|
| Horizon Blauw | `#009cdc` | Primary, headlines, nav, backgrounds |
| Notenpasta / Crème | `#fff0d8` | Earth, backgrounds, text on blue |
| Organisch Groen | `#60d553` | CTA buttons, accent badges only |
| Lichtblauw | `#b8e8f5` | Gradient midpoint |
| Zwart | `#000000` | Body text |
| Wit | `#ffffff` | Cards, overlays |

### Typography
| Role | Font | Size | Weight | Letter-spacing |
|------|------|------|--------|---------------|
| H1/H2 headlines | Clarendon | clamp(3.4rem, 10.5vw, 9.5rem) | 800 | -0.01em |
| Section titles | Clarendon | clamp(2rem, 5vw, 4rem) | 800 | -0.01em |
| Stats numbers | Clarendon | clamp(2.2rem, 4.5vw, 4rem) | 800 | -0.02em |
| Mission statement | Clarendon | clamp(1.4rem, 3.5vw, 3rem) | 800 | 0 |
| Body text | DM Sans | 1rem (16px) | 400 | 0 |
| Labels | DM Sans | 12px | 500 | 0.13em |
| Logo underline | DM Sans | 8px | 500 | 0.33em |
| Nav links | DM Sans | 14px | 500 | 0.05em |

### Spacing
```
--s1: 8px   --s2: 16px   --s3: 24px   --s4: 32px
--s5: 48px  --s6: 64px   --s8: 96px   --s10: 128px
```

### Border radius
- Buttons: `99px` (full pill)
- Cards: `12px`
- Labels: `99px`
- Value circles: `50%`

---

## Assets

| File | Usage |
|------|-------|
| `assets/fonts/Clarendon-ExtraBold.otf` | Primary display font (Clarendon Extra Bold 800) |
| `assets/scraped/images/merken/horizon/coverMuur-min.png` | Horizon product hero image |
| `assets/scraped/images/merken/horizon/horizonpasta/*.png` | Individual product images |
| `assets/scraped/images/merken/monki/monPotMuur-min.png` | Monki brand image |
| `assets/scraped/images/merken/jori/FrontCover_T-min.png` | Jori brand image |
| `assets/scraped/images/merken/krekeltje/krekeltjeImage-min.png` | Krekeltje brand image |
| `assets/images/EUBio.jpg` | EU Biologisch certification badge |
| `assets/images/FaviconRound.png` | Favicon |

**External dependencies:**
- Google Fonts: DM Sans (400, 500, 700)
- SunCalc v1.9.0: `https://cdn.jsdelivr.net/npm/suncalc@1.9.0/suncalc.min.js`

---

## Files in this bundle

| File | Description |
|------|-------------|
| `Horizon Natuurvoeding - Website.html` | Complete high-fidelity prototype (single file) |
| `colors_and_type.css` | Full design system tokens (colors, typography, spacing, shadows) |
| `README.md` | This document |

---

## Notes for developer

1. **Clarendon font**: Load via `@font-face` from `assets/fonts/Clarendon-ExtraBold.otf`. This is NOT on Google Fonts — the .otf file must be served locally.
2. **Horizon cut JS**: This is the most complex part. The `adjustHorizonCut()` function must run AFTER fonts are loaded (Clarendon affects letter sizes). Use `document.fonts.ready.then()` or a timeout.
3. **Sun animation**: The canvas element sits on top of the sky gradient as an overlay. It uses `rgba` fills so the gradient shows through.
4. **Logo SVG**: The HORIZON wordmark is an inline SVG (vector paths, not a font). It's included directly in the HTML — copy the `<svg>` block.
5. **Fade-on-scroll**: Uses IntersectionObserver with `threshold: 0.15` and `rootMargin: '0px 0px -60px 0px'`.
