# Brand Design — [Business Name]

Fill this in from the Client Brief at project kickoff.
Sits on top of base-design.md. Never overrides spacing, tokens, or breakpoints.

---

## Colors

Add these to the :root block in assets/css/styles.css, after the base token block.

```css
:root {
  --brand-primary:   [hex];   /* CTA buttons, links, key accents */
  --brand-secondary: [hex];   /* hover states, highlights */
  --brand-dark:      [hex];   /* headings, dark backgrounds, nav */
  --brand-light:     [hex];   /* light section backgrounds */
  --white:           #ffffff;
  --text-body:       [hex];   /* main body copy color */
  --border:          [hex];   /* dividers, card borders */
}
```

---

## Typography

- **Heading font:** [Font name] — weights [e.g. 700, 900]
- **Body font:** [Font name] — weights [e.g. 400, 500]

Paste Google Fonts link tag in the <head> of every page, before the stylesheet link:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="[paste full Google Fonts URL here]" rel="stylesheet">
```

---

## Tone of voice

- [e.g. Friendly and direct — talks like a tradie, not a marketer]
- [e.g. First person "I" not "we" — sole trader]
- [e.g. Aussie spelling throughout — organise, colour, honour]
- [e.g. CTAs: "Get a free quote", "Give us a ring", "Send us a message"]

---

## Logo

- Main file: assets/images/logo.png
- Light variant (for dark backgrounds): assets/images/logo-light.png (if supplied)
- Used in: nav top-left, footer, OG image

---

## Business details

- **Business name:** [Name]
- **Primary suburb:** [Suburb]
- **Phone:** [Number]
- **WhatsApp number:** [Number with country code, e.g. 61412345678]
- **Email:** [Email]
- **ABN:** [ABN]
- **Service area:** [List of suburbs]
- **Google Business Profile:** [URL]
