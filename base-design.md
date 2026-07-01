# FreeRange Websites — Base Design System

System-wide structural rules. Copy unchanged into every new client repo.
Never edit this file during a client build — brand overrides go in brand-design.md.
Last updated: July 2026 (v1.0)

---

## 1. Open Props — Import

Add this as the FIRST line of assets/css/styles.css on every client site:

```css
@import "https://unpkg.com/open-props";
```

---

## 2. Semantic Token Block

Add this entire :root block to assets/css/styles.css immediately after the Open Props import.
These are the ONLY sizing, spacing, shadow, radius, and z-index values permitted.
Never use arbitrary px values for font-size, padding, or margin.

```css
:root {

  /* ── TYPE SCALE ── */
  --text-xs:    clamp(0.75rem, 1.5vw, 0.875rem);  /* Labels, tags, fine print */
  --text-sm:    clamp(0.875rem, 1.8vw, 1rem);      /* Captions, secondary body */
  --text-base:  clamp(1rem, 2vw, 1.125rem);        /* Main body copy, list items */
  --text-md:    clamp(1.1rem, 2.5vw, 1.35rem);     /* Lead paragraphs, subtitles */
  --text-lg:    clamp(1.25rem, 3vw, 1.75rem);      /* H3, card headings */
  --text-xl:    clamp(1.5rem, 4vw, 2.25rem);       /* H2, section headings */
  --text-2xl:   clamp(2rem, 5vw, 3rem);            /* H1, hero headings */
  --text-3xl:   clamp(2.5rem, 6vw, 4rem);          /* Hero display / jumbo headline */

  /* ── SPACING SCALE ── */
  --space-1:    clamp(4px, 1vw, 8px);              /* Tight gaps, icon spacing */
  --space-2:    clamp(8px, 2vw, 16px);             /* Inner element gaps */
  --space-3:    clamp(16px, 3vw, 24px);            /* Card padding, container side pad */
  --space-4:    clamp(24px, 4vw, 40px);            /* Component padding */
  --space-5:    clamp(40px, 6vw, 64px);            /* Section padding vertical */
  --space-6:    clamp(64px, 8vw, 100px);           /* Large section padding */
  --space-7:    clamp(80px, 10vw, 140px);          /* Hero padding vertical */

  /* ── MAX WIDTHS ── */
  --width-content: 1160px;   /* Standard container — most sections */
  --width-wide:    1440px;   /* Expansive sections — stats bars, logo strips */
  --width-prose:   680px;    /* Body text, hero subtitles, narrow columns */
  /* Full bleed (hero, image bands, colour sections) = no max-width on section,
     .container inside still constrains content to --width-content */

  /* ── BORDER RADIUS ── */
  --radius-btn:    6px;      /* Buttons */
  --radius-card:   12px;     /* Cards, panels */
  --radius-badge:  50px;     /* Tags, pills, badges */
  --radius-input:  8px;      /* Form inputs */
  --radius-round:  9999px;   /* Fully round — avatars, icon circles */
  --radius-lg:     20px;     /* larger cards, panels */
  --radius-xl:     28px;     /* feature cards, prominent panels */

  /* ── SHADOWS ── */
  --shadow-card:   0 2px 12px rgba(0,0,0,0.08);          /* Default card elevation */
  --shadow-card-hover: 0 8px 28px rgba(0,0,0,0.14);      /* Card on hover */
  --shadow-nav:    0 2px 16px rgba(0,0,0,0.10);          /* Sticky nav */
  --shadow-modal:  0 16px 48px rgba(0,0,0,0.22);         /* Modals, drawers */
  --shadow-btn:    0 2px 8px rgba(0,0,0,0.14);           /* Button depth */

  /* ── Z-INDEX SCALE ── */
  --z-base:        0;        /* Default flow */
  --z-raised:      10;       /* Cards on hover, tooltips */
  --z-dropdown:    100;      /* Dropdowns, select menus */
  --z-sticky:      200;      /* Sticky nav */
  --z-overlay:     300;      /* Page overlays, backdrops */
  --z-modal:       400;      /* Modals, drawers */
  --z-toast:       500;      /* Notifications, toasts */

  /* ── LINE HEIGHTS ── */
  --leading-tight:   1.1;    /* Large display headings, hero H1 */
  --leading-snug:    1.3;    /* H2, H3, card headings */
  --leading-normal:  1.6;    /* Body copy */
  --leading-relaxed: 1.8;    /* Long-form text, blog content */

  /* ── LETTER SPACING ── */
  --tracking-tight:  -0.04em;  /* Large hero headings */
  --tracking-normal:  0;       /* Body copy */
  --tracking-wide:    0.04em;  /* Tags, badges, labels */
  --tracking-wider:   0.08em;  /* Uppercase section labels */

  /* ── TRANSITIONS ── */
  --transition-fast:   all 0.15s ease;           /* Hover colour changes */
  --transition-base:   all 0.25s ease;           /* Standard hover effects */
  --transition-slow:   all 0.4s ease;            /* Panel opens, expands */
  --transition-spring: all 0.35s cubic-bezier(0.34, 1.56, 0.64, 1); /* Springy pop */

  /* ── BORDERS ── */
  --border-subtle:  1px solid rgba(0,0,0,0.08);   /* Card borders, dividers */
  --border-medium:  1px solid rgba(0,0,0,0.15);   /* Input borders */
  --border-strong:  2px solid currentColor;        /* Focus rings, accents */

  /* ── ASPECT RATIOS ── */
  --ratio-hero:     16 / 7;   /* Hero images */
  --ratio-card:     4 / 3;    /* Card images */
  --ratio-thumb:    1 / 1;    /* Square thumbnails, avatars */
  --ratio-wide:     21 / 9;   /* Cinematic banner images */

  /* ── BUTTON SIZING ── */
  --btn-sm-padding:   6px 14px;     /* Small — inline CTAs, tags */
  --btn-sm-font:      var(--text-xs);
  --btn-md-padding:   12px 28px;    /* Medium — default button */
  --btn-md-font:      var(--text-sm);
  --btn-lg-padding:   16px 40px;    /* Large — primary hero CTA */
  --btn-lg-font:      var(--text-base);

}
```

---

## 3. Breakpoints

Only these two breakpoints are permitted. Never invent new ones.

| Name   | Value           | Use for                              |
|--------|-----------------|--------------------------------------|
| Tablet | max-width 960px | Grid reflow, sidebar stacking        |
| Mobile | max-width 640px | Nav collapse, single column, spacing |

---

## 4. Container Rules

```css
.container        {max-width: var(--width-content);margin: 0 auto;padding: 0 var(--space-3);}
.container--wide  {max-width: var(--width-wide);margin: 0 auto;padding: 0 var(--space-3);}
.container--prose {max-width: var(--width-prose);margin: 0 auto;padding: 0 var(--space-3);}
```

Full bleed sections (hero, image bands, colour sections):
- No max-width on the section itself
- Always place a .container inside to constrain the text content

---

## 5. Scroll Reveal Animations

Use Intersection Observer for all section scroll animations. GSAP is reserved for
hero entrance animations only. Add this script once, before </body> on every page.

```html
<script>
  const reveals = document.querySelectorAll('[data-reveal]');
  const io = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('revealed');
        io.unobserve(e.target);
      }
    });
  }, {threshold: 0.15});
  reveals.forEach(el => io.observe(el));
</script>
```

Add this CSS block to styles.css:

```css
[data-reveal]                {opacity: 0;transition: opacity 0.6s ease, transform 0.6s ease;}
[data-reveal="fade-up"]      {transform: translateY(32px);}
[data-reveal="fade-in"]      {transform: none;}
[data-reveal="slide-left"]   {transform: translateX(-40px);}
[data-reveal="slide-right"]  {transform: translateX(40px);}
[data-reveal].revealed       {opacity: 1;transform: none;}
```

Usage in HTML — add data-reveal to any section or element:

```html
<section data-reveal="fade-up">...</section>
<div data-reveal="slide-left">...</div>
<div data-reveal="fade-in">...</div>
```

Stagger child elements by adding inline style delay:

```html
<div data-reveal="fade-up" style="transition-delay: 0.1s">First</div>
<div data-reveal="fade-up" style="transition-delay: 0.2s">Second</div>
<div data-reveal="fade-up" style="transition-delay: 0.3s">Third</div>
```

---

## 6. Section Dividers

Pick from this library. Never create custom divider geometry from scratch.
All dividers go between two sections. The divider element sits inside the upper section.

---

### D1 — Diagonal / Top-Left Rise
Background of lower section appears to rise from left to right.
```html
<div class="divider divider--diag-left"></div>
```
```css
.divider {position: relative;height: 80px;overflow: hidden;}
.divider--diag-left  {clip-path: polygon(0 0, 100% 100%, 100% 100%, 0 100%);}
```

---

### D2 — Diagonal / Top-Right Rise
Background rises from right to left.
```html
<div class="divider divider--diag-right"></div>
```
```css
.divider--diag-right {clip-path: polygon(0 100%, 100% 0, 100% 100%, 0 100%);}
```

---

### D3 — SVG Wave / Gentle
Soft flowing wave between sections.
```html
<div class="divider divider--wave-gentle">
  <svg viewBox="0 0 1440 80" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M0,40 C360,80 1080,0 1440,40 L1440,80 L0,80 Z" fill="var(--next-section-bg)"/>
  </svg>
</div>
```
```css
.divider--wave-gentle {line-height: 0;}
.divider--wave-gentle svg {display: block;width: 100%;height: 80px;}
```

---

### D4 — SVG Wave / Sharp
More pronounced wave with higher amplitude.
```html
<div class="divider divider--wave-sharp">
  <svg viewBox="0 0 1440 100" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M0,60 C240,100 480,0 720,60 C960,100 1200,0 1440,60 L1440,100 L0,100 Z" fill="var(--next-section-bg)"/>
  </svg>
</div>
```
```css
.divider--wave-sharp {line-height: 0;}
.divider--wave-sharp svg {display: block;width: 100%;height: 100px;}
```

---

### D5 — Curve / Convex (Bulges Down)
Lower section has a convex curved top edge.
```html
<div class="divider divider--curve-convex"></div>
```
```css
.divider--curve-convex {height: 80px;border-radius: 0 0 50% 50% / 0 0 100% 100%;background: var(--next-section-bg);}
```

---

### D6 — Curve / Concave (Scoops Up)
Lower section has a concave curved top edge.
```html
<div class="divider divider--curve-concave"></div>
```
```css
.divider--curve-concave {height: 80px;border-radius: 50% 50% 0 0 / 100% 100% 0 0;background: var(--current-section-bg);}
```

---

### D7 — Torn Paper / Rough Edge
Organic jagged edge between sections.
```html
<div class="divider divider--torn">
  <svg viewBox="0 0 1440 60" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M0,30 L48,18 L96,42 L144,20 L192,38 L240,14 L288,36 L336,22 L384,40 L432,16 L480,34 L528,10 L576,32 L624,24 L672,44 L720,18 L768,38 L816,12 L864,30 L912,20 L960,42 L1008,16 L1056,36 L1104,22 L1152,40 L1200,18 L1248,34 L1296,10 L1344,28 L1392,20 L1440,30 L1440,60 L0,60 Z" fill="var(--next-section-bg)"/>
  </svg>
</div>
```
```css
.divider--torn {line-height: 0;}
.divider--torn svg {display: block;width: 100%;height: 60px;}
```

---

### D8 — Angled Image Bleed
Full-width image that angles into the next section. Place between hero and first section.
```html
<div class="divider divider--img-bleed">
  <img src="/assets/images/[image].jpg" alt="">
</div>
```
```css
.divider--img-bleed {position: relative;height: 400px;overflow: hidden;clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%);}
.divider--img-bleed img {width: 100%;height: 100%;object-fit: cover;}
```

---

### D9 — Gradient Fade
Smooth colour dissolve between sections. No shape — purely a colour transition.
```html
<div class="divider divider--fade" style="--from: #ffffff; --to: #f1f5f9;"></div>
```
```css
.divider--fade {height: 120px;background: linear-gradient(to bottom, var(--from), var(--to));}
```

---

## 7. Common Mistakes — Catch Before Finishing

| Wrong | Right |
|---|---|
| padding: 80px 20px | padding: var(--space-6) var(--space-3) |
| font-size: 32px | font-size: var(--text-2xl) |
| margin-bottom: 48px | margin-bottom: var(--space-5) |
| box-shadow: 0 4px 12px rgba(0,0,0,0.1) | box-shadow: var(--shadow-card) |
| border-radius: 8px on a card | border-radius: var(--radius-card) |
| z-index: 999 | z-index: var(--z-modal) |
| transition: all 0.3s ease | transition: var(--transition-base) |
| padding: 150px 0 80px 25px | padding: var(--space-7) 0 var(--space-6) var(--space-3) |
| Custom divider geometry | Pick from divider library (Section 6) |
| Custom scroll animation | Use data-reveal pattern (Section 5) |
