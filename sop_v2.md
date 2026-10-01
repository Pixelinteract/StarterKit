# FreeRange Websites — Client Site Build SOP
**Version 1.2 — June 2026**
*Standard Operating Procedure for building, structuring, and deploying client websites.*
*v1.2: Added CSS formatting standard (single-line rules).*

---

## Overview

Client sites are static HTML websites hosted on Cloudflare Pages, deployed via GitHub. Each site is built using Claude AI and follows a consistent folder structure and shared component methodology. This document covers everything needed to build a new client site from scratch.

---

## Client Intake & Onboarding Automation

This section covers everything from the moment a client pays through to having all their assets ready to build with.

---

### Tools Used

| Tool | Purpose | Cost |
|---|---|---|
| Stripe | Payment collection | Per transaction |
| Make.com | Automation (replaces Zapier) | Free tier |
| Google Drive | Asset collection (logo, photos) | Free |
| Tally | Text intake form | Free tier |
| Gmail | Welcome email delivery | Free |
| Google Sheets | Client log | Free |

> **Why Make.com over Zapier?** Make's free tier includes multi-step scenarios and 1,000 operations/month. Zapier free is limited to two-step Zaps only, which isn't enough for this flow.

---

### The Intake Flow — Overview

```
Client pays via Stripe
  │
  ├── Stripe redirects client → /onboarding.html (immediate, while engaged)
  │
  └── Make.com fires in background:
        1. Creates Google Drive folder → [clientname]
        2. Sends welcome email → with Tally form link + Drive folder link
        3. Logs new client row → Google Sheets CRM
```

The Stripe redirect gets the form in front of the client at peak engagement. The welcome email is a backup — for when they close the tab, get interrupted, or need to find the Drive link again.

---

### Step 1 — Stripe Setup

In your Stripe payment link settings:

- Enable **phone number** field at checkout (useful before the Tally form comes back)
- Set **success URL** to `https://www.freerangewebsites.com.au/onboarding.html` — client lands on the intake page the moment payment clears

---

### Step 2 — Tally Form (Text Only)

Tally collects all text-based intake information. No file uploads — everything file-based goes to Google Drive instead. This keeps Tally on the free tier permanently.

**Fields to include:**

| Field | Type | Notes |
|---|---|---|
| Full name | Short text | Contact person |
| Business name | Short text | |
| Phone number | Short text | May already have from Stripe |
| Email address | Email | |
| Business address or service area | Short text | e.g. "Dandenong — no shopfront" |
| ABN | Short text | Required for .com.au domains |
| Primary suburb | Short text | Main area targeted for SEO |
| Secondary service areas | Long text | "List all suburbs you service" |
| Services offered | Long text | Ask them to list each service |
| "Why choose you" | Long text | USP — what makes them different |
| Testimonials | Long text | Optional — paste any they have |
| Do you own a domain? | Yes / No | |
| Domain name (if yes) | Short text | e.g. peteselectrical.com.au |
| Domain registrar (if yes) | Short text | e.g. VentraIP, GoDaddy |
| Need a domain registered? | Yes / No | Triggers $35/yr charge |
| Brand colours | Short text | Hex codes if known — otherwise describe |
| Anything else we should know | Long text | Open field |

**Tally confirmation message** (shown after submit):

> Thanks — we've got everything. Check your inbox — we've sent you a link to your private upload folder for your logo and photos.
>
> We'll be in touch once we have everything. Most sites are live within 7–21 business days from when we receive all your assets.

**Important:** Do NOT redirect to book-a-call on form submit. Use Tally's built-in ending screen above. The book-a-call link is delivered softly via the welcome email — forcing it immediately after a long form feels like another task, not a helpful offer.

---

### Step 3 — Make.com Scenario

Build this as a single scenario in Make.com triggered by a new Stripe payment.

**Trigger:** Stripe → Watch Events → `payment_intent.succeeded` (or `checkout.session.completed`)

**Actions in order:**

1. **Google Drive → Create a Folder**
   - Name: `[customer name]` — use the name field from the Stripe event
   - Location: inside a parent `FRW Clients` folder in your Drive
   - Set sharing to *Anyone with the link can upload*

2. **Gmail → Send an Email**
   - To: customer email from Stripe
   - Subject: `You're in — here's what happens next`
   - Body: see welcome email template below
   - Inject the Drive folder URL from Step 1 dynamically

3. **Google Sheets → Add a Row**
   - Sheet: `FRW Client CRM`
   - Columns: Date, Name, Email, Phone, Plan, Drive Folder URL, Tally Submitted (blank — fill manually), Status

**Make.com free tier note:** Each action = 1 operation. This scenario uses 3 operations per new client. At 30 clients/month that's 90 operations — well within the 1,000/month free limit.

---

### Welcome Email Template

> **Subject:** You're in — here's what happens next
>
> Hey [first name],
>
> Payment received — welcome to Free Range Websites.
>
> **Step 1 — Fill in your details (5 mins)**
> If you haven't already, complete your intake form here:
> [Tally form link]
>
> **Step 2 — Upload your logo and photos**
> Drop them into your private folder — logo, work photos, anything you want on the site:
> [Google Drive folder link — injected by Make]
>
> Once we have both, we'll get started reviewing everything. Most sites are live within 7–21 business days from when we receive all your assets.
>
> ---
>
> **Want to have a quick chat first?**
>
> Once you've submitted the form, we'll reach out to go over the details — but if you'd rather lock in a time now, you're welcome to grab a spot on our calendar:
> [freerangewebsites.com.au/book-a-call.html]
>
> No pressure either way — we'll be in touch regardless.
>
> Any questions, just reply to this email.
>
> [Son's name]
> Free Range Websites

Two required steps up top, book-a-call below the divider as a soft optional offer. Tradies won't feel obligated — but those who want to chat can act on it immediately.

---

### Step 4 — Compiling the Client Brief

Once the Tally form is submitted and photos are in Drive, compile everything into a **Client Brief** before handing to Claude. This is the single source of truth for the build.

Use this template — copy it into a new Google Doc named `[clientname]-brief`:

```
CLIENT BRIEF — [Business Name]
Date: [DD/MM/YYYY]
Plan: Starter / Standard
Domain: [domain or TBC]

BUSINESS
Name:
ABN:
Phone:
Email:
Address / Service area:
Primary suburb:
Secondary service areas:

SERVICES
- [List each service on its own line]

WHY CHOOSE THEM
[Paste their USP response from Tally]

TESTIMONIALS
[Paste any they provided — or "None supplied"]

PAGES REQUIRED
1. Home
2. Services
3. Contact
[4. About — Standard only]
[5. [Suburb]-[trade] landing page — Standard only]

ASSETS
Logo: [filename or "pending"]
Photos: [Drive folder link]
OG image: [filename or "to create"]
Brand colours: [hex codes or description]

NOTES
[Anything from the "anything else" field, or follow-up notes]
```

---

### Step 5 — Building with Claude

With the Client Brief compiled and assets in Drive:

1. Open a new Claude chat (or use the FRW Project if set up)
2. Attach or paste the **Client Brief**
3. Attach or paste the **SOP** (or rely on it being in the Project context)
4. Download the logo from Drive and attach it directly to the chat
5. Download 2–3 of the best photos from Drive and attach them

Then use this standardised build prompt:

```
Build a complete client website using the FreeRange Websites SOP and 
the attached Client Brief.

Generate all files:
- index.html, [services.html], [contact.html], [additional pages]
- assets/css/styles.css
- assets/js/includes.js
- assets/js/home.js
- assets/includes/header.html
- assets/includes/footer.html

Follow all folder structure, SEO, head template, JSON-LD, and component 
rules in the SOP exactly. Write in Aussie English throughout. 
Sell outcomes — leads, calls, rankings — not aesthetics.

Client is on the [Starter / Standard] plan.
Pages required: [list from brief]
```

Claude generates all files. Drop them into Phoenix Code, review in live preview, push to GitHub.

---

### Intake Checklist — Per New Client

- [ ] Stripe payment received
- [ ] Make scenario fired — Drive folder created, welcome email sent, CRM row added
- [ ] Tally form submitted — mark in Google Sheets
- [ ] Photos uploaded to Drive folder
- [ ] Client Brief compiled (Google Doc)
- [ ] Logo downloaded from Drive → renamed `logo.png`
- [ ] Photos reviewed — select best 3–5 for build
- [ ] Claude build initiated with Brief + assets attached
- [ ] Draft site reviewed in Phoenix Code
- [ ] Draft sent to client for review

---

## FreeRange Websites — Live Pages Reference

Pages that exist on freerangewebsites.com.au and their purpose:

| Page | URL | Indexed | Purpose |
|---|---|---|---|
| Home | `/index.html` | ✅ Yes | Main marketing page |
| Get Started | `/get-started.html` | ✅ Yes | Sign-up funnel → Stripe |
| Contact | `/contact.html` | ✅ Yes | General enquiries (Tally embed) |
| Book a Call | `/book-a-call.html` | ✅ Yes | Calendly embed — prospects + post-onboarding clients |
| Onboarding | `/onboarding.html` | ❌ No | Post-payment intake form (Tally embed) — Stripe redirects here |
| Terms | `/terms.html` | ✅ Yes | Terms and Conditions |
| Privacy | `/privacy.html` | ✅ Yes | Privacy Policy |

**Flow after payment:**
Stripe → `/onboarding.html` → client completes Tally form → Tally ending screen → Make welcome email delivers Drive folder link + soft book-a-call invite

---

## Folder Structure

Every client site must follow this exact structure:

```
/
├── index.html
├── contact.html
├── terms.html
├── privacy.html
├── [additional-pages].html       ← e.g. services.html, about.html
├── [suburb-niche]/
│   └── index.html                ← SEO suburb landing page (Standard plan+)
└── assets/
    ├── css/
    │   └── styles.css            ← ALL styles for the site
    ├── js/
    │   ├── includes.js           ← runs on every page (nav inject + scroll + mobile)
    │   └── home.js               ← home page only (ticker, FAQ, scroll reveal etc)
    ├── includes/
    │   ├── header.html           ← shared nav partial
    │   └── footer.html           ← shared footer partial
    └── images/
        ├── logo.png
        ├── favicon.png
        ├── apple-touch-icon.png
        ├── og-image.jpg          ← 1200×630px for social sharing
        └── [all other images]
```

### Rules
- All image paths must be **absolute from root** — always `/assets/images/filename.jpg`, never `images/filename.jpg` or `../images/filename.jpg`. Relative paths break on any page not at the root.
- Maximum **5 pages** per site (Starter: 3 pages, Standard: 5 pages).
- Suburb landing pages go in their own subfolder with an `index.html` — e.g. `/plumber-dandenong/index.html`.

---

## Shared Components — How It Works

Nav and footer are written once and injected into every page at runtime via `includes.js`. This avoids duplicating markup across files.

### Page Shell

Every HTML page must include these two placeholder divs and the script tag — in this exact order:

```html
<body>
  <div id="site-header"></div>

  <!-- page content here -->

  <div id="site-footer"></div>
  <script src="/assets/js/includes.js"></script>
  <!-- page-specific script only if needed, e.g.: -->
  <script src="/assets/js/home.js"></script>
</body>
```

### `includes.js` Responsibilities
- Fetches and injects `header.html` into `#site-header`
- Fetches and injects `footer.html` into `#site-footer`
- Sets active nav link based on `window.location.pathname`
- Handles nav scroll hide/show + progress bar
- Handles mobile nav toggle (hamburger)
- Handles smooth anchor scrolling

### `home.js` Responsibilities (home page only)
- Hero testimonial ticker animation
- Scroll reveal (`.rv` elements)
- FAQ accordion toggle

### Important: JS runs after DOM
All nav-dependent JS (scroll, mobile toggle, active state) runs **inside the header fetch callback** — not at the top level. This ensures the nav exists in the DOM before the JS tries to reference it.

---

## `header.html` — What Goes In It

- `.nav-wrap` div containing the `nav.nav-pill`
- Logo image linked to `/`
- Nav links — use **absolute paths with root-relative anchors** for home page sections:
  - `href="/#how-it-works"` not `href="#how-it-works"`
  - `href="/#pricing"` not `href="#pricing"`
- Mobile nav div `#mobNav` — no `onclick` attributes on links (handled by `includes.js`)
- "Get started" CTA links to `/#pricing`
- Contact link: `/contact.html`

### Active Nav State
`includes.js` checks `window.location.pathname` after injection and adds `.active` to the matching link. No manual work needed per page.

---

## `footer.html` — What Goes In It

- `<footer class="footer">` with logo, nav links, copyright, ABN
- Acknowledgement of Country bar
- WhatsApp click-to-chat button (`.wa-float`)
- All footer nav links use `.html` extensions: `/contact.html`, `/terms.html`, `/privacy.html`
- Logo image: `/assets/images/logo.png`

---

## CSS — One File, Linked Per Page

All styles live in `assets/css/styles.css`. Every page links to it in the `<head>`:

```html
<link rel="stylesheet" href="/assets/css/styles.css">
```

**Do not use inline styles in the body.** If an element needs custom styles, add a class to `styles.css`. Inline styles are not maintainable across a multi-page site.

The CSS link goes in `<head>` — not in the includes — so styles are available before the page renders, preventing any flash of unstyled content.

---

## CSS Formatting Standard

All CSS rules must be written as **single-line rules** — one rule per line with all properties on the same line as the selector.

**Correct:**
```css
.nav-wrap {position: fixed;top: 0;left: 0;right: 0;z-index: 100;}
.hero {position: relative;height: 95vh;min-height: 480px;}
.btn--primary {background: var(--accent);color: var(--white);border-color: var(--accent);}
```

**Not this:**
```css
.nav-wrap {
  position: fixed;
  top: 0;
  left: 0;
}
```

Media queries keep their own opening and closing braces on separate lines, but all rules inside follow the same single-line format:

```css
@media (max-width: 640px) {
  .hero {height: auto;min-height: 100svh;}
  .foot-grid {grid-template-columns: 1fr;}
  .foot-btm {flex-direction: column;gap: 6px;text-align: center;}
}
```

**Why:** Easier to scan and find/replace specific rules. Consistent with how Claude generates snippets for this project. Reduces visual noise in long stylesheets.

**To reformat an existing file to this standard:** Upload the HTML file to Claude and ask it to collapse all CSS rules to single-line format. Claude will run a Python script and return the reformatted file ready to use.

---

## `<head>` Template — Per Page

The following is required in every page's `<head>`. Items marked *unique* must be changed per page. Items marked *shared* are copy-pasted identically.

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- PRIMARY SEO — UNIQUE PER PAGE -->
  <title>[Service] in [Suburb] | [Business Name]</title>
  <meta name="description" content="[Unique description ~155 chars targeting local search intent]">
  <meta name="robots" content="index, follow">
  <link rel="canonical" href="https://www.[clientdomain].com.au/[page]">

  <!-- FAVICON — SHARED -->
  <link rel="icon" type="image/png" href="/assets/images/favicon.png">
  <link rel="apple-touch-icon" href="/assets/images/apple-touch-icon.png">

  <!-- OPEN GRAPH — UNIQUE PER PAGE -->
  <meta property="og:type" content="website">
  <meta property="og:site_name" content="[Business Name]">
  <meta property="og:title" content="[Same as title tag]">
  <meta property="og:description" content="[Same as meta description]">
  <meta property="og:url" content="https://www.[clientdomain].com.au/[page]">
  <meta property="og:image" content="https://www.[clientdomain].com.au/assets/images/og-image.jpg">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:locale" content="en_AU">

  <!-- TWITTER CARD — UNIQUE PER PAGE -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="[Same as title tag]">
  <meta name="twitter:description" content="[Same as meta description]">
  <meta name="twitter:image" content="https://www.[clientdomain].com.au/assets/images/og-image.jpg">

  <!-- GOOGLE ANALYTICS — SHARED (replace ID) -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-XXXXXXXXXX');
  </script>

  <!-- JSON-LD STRUCTURED DATA — UNIQUE PER PAGE -->
  <!-- See JSON-LD section below -->

  <!-- FONTS — SHARED -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Encode+Sans+Condensed:wght@400;500;600;700;800;900&family=Encode+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">

  <!-- STYLESHEET — SHARED -->
  <link rel="stylesheet" href="/assets/css/styles.css">
</head>
```

---

## JSON-LD Structured Data — Per Page

Every page gets its own JSON-LD. The `@graph` on `index.html` includes Organization, WebSite, WebPage, and Service nodes. Inner pages only need WebPage (and LocalBusiness where relevant).

### Home page (`index.html`)
Include: `Organization`, `WebSite`, `WebPage`, `Service` with pricing offers.

### Inner pages (`contact.html`, `services.html` etc)
Include: `WebPage` only, referencing the Organisation by `@id`.

### Suburb landing pages
Include: `LocalBusiness` with `address`, `areaServed`, `geo` (lat/lng), and `WebPage`.

---

## SEO Rules — Every Client Site

- Every page must have a **unique** `<title>` and `<meta name="description">`
- Title format: `[Primary Service] in [Suburb] | [Business Name]`
- Meta description: ~155 characters, natural language, includes suburb and service
- `<link rel="canonical">` on every page pointing to its own URL
- Home page targets the client's primary suburb
- Suburb landing pages target secondary service areas
- Every image must have descriptive `alt` text
- Use `LocalBusiness` schema on suburb landing pages with `areaServed`
- `sitemap.xml` submitted via Google Search Console (Standard plan+)

---

## Copywriting Standards

- Write in **Aussie English**: "Organise a quote" not "Schedule an appointment", "Colour" not "Color", "Honour" not "Honor"
- Sell outcomes, not aesthetics: leads, calls, Google rankings — not design
- Plain-speaking tone — no corporate fluff
- Use the client's actual suburb name in copy, not just the city
- CTAs: "Get a free quote", "Give us a ring", "Send us a message"

---

## Image Checklist

Before deploying, confirm these files exist in `/assets/images/`:

- [ ] `logo.png` — transparent background preferred
- [ ] `favicon.png` — 32×32px minimum
- [ ] `apple-touch-icon.png` — 180×180px
- [ ] `og-image.jpg` — exactly 1200×630px, includes business name

---

## Deployment — Cloudflare Pages via GitHub

1. Create a new **private GitHub repo** for the client — naming convention: `[clientname]`
2. Push all files to `main` branch
3. Connect repo to Cloudflare Pages
4. Set build command to: *(none — static site, no build step)*
5. Set output directory to: `/` (root)
6. Add custom domain in Cloudflare Pages dashboard
7. Point client's domain DNS to Cloudflare (provide client with nameservers or A/CNAME records)

---

## New Page Checklist

When adding any new page to a client site:

- [ ] Copy the `<head>` block from an existing page
- [ ] Update title, description, canonical, OG tags, JSON-LD for the new page
- [ ] Add `<div id="site-header"></div>` at top of body
- [ ] Add `<div id="site-footer"></div>` before closing `</body>`
- [ ] Add `<script src="/assets/js/includes.js"></script>` before `</body>`
- [ ] Add page-specific script tag only if needed
- [ ] Add the new page link to `header.html` and `footer.html` if it belongs in the nav
- [ ] All internal links use `.html` extension and absolute root-relative paths
- [ ] All image paths use `/assets/images/` prefix

---

## Common Mistakes to Avoid

| Mistake | Correct Approach |
|---|---|
| Relative image paths (`images/logo.png`) | Always use `/assets/images/logo.png` |
| Inline styles in the body | Add a class to `styles.css` |
| Anchor links without `/#` on inner pages | Use `/#pricing` not `#pricing` |
| Nav/footer links without `.html` extension | Always use `/contact.html` not `/contact` |
| Duplicate title tags across pages | Every page gets a unique title |
| Missing canonical tag | Every page needs `<link rel="canonical">` |
| `onclick="toggleMob()"` on mobile nav links | Handled by `includes.js` event listener — no onclick needed |
| JS referencing DOM elements before includes inject | All nav JS runs inside the header fetch callback |
| Multi-line CSS rules | Always use single-line format: `.class {property: value;property: value;}` |

---

*Last updated: June 2026 (v1.2) — update this document whenever the build process changes.*
