# FreeRange Websites — Client Site

This file is auto-loaded by Claude Code at the start of every session.
For a new client, copy this file into the repo root and fill in the
**Client setup** section below.

---

## Session start — read these first

Before doing anything else, fetch and read these files from StarterKit:

- SOP: https://raw.githubusercontent.com/Pixelinteract/StarterKit/main/sop_v2.md
- Base design: https://raw.githubusercontent.com/Pixelinteract/StarterKit/main/base-design.md
- Brand design: brand-design.md (in this repo — read locally)

Do not proceed with any build task until all three are loaded.

---

## New client kickoff checklist

1. [ ] Copy CLAUDE.md and brand-design.md from StarterKit into this repo root
2. [ ] Fill in the Client setup section below
3. [ ] Fill in brand-design.md from the compiled Client Brief
4. [ ] Confirm folder structure: flat root, assets/css, assets/images, assets/js, assets/includes
5. [ ] Add Open Props import as first line of assets/css/styles.css
6. [ ] Paste full :root token block from base-design.md into styles.css below the import

---

## Client setup (edit per project)

- **Business name:** [Business Name]
- **Domain:** [domain.com.au]
- **GitHub repo:** Pixelinteract/frw-[clientname] (branch: main)
- **Hosting:** Cloudflare Pages (no build step, root = /)
- **Plan:** Starter / Standard
- **Local repo path:** /Users/basil/Desktop/FreeRange/frw-[clientname]

---

## Golden rules (apply to every site)

### 1. Design files load in this order — always follow both
- `base-design.md` (fetched from StarterKit) — structural rules, tokens, breakpoints. Never override.
- `brand-design.md` (in this repo) — this client's colors, fonts, tone. Sits on top of base rules only.

### 2. Sizing and spacing — use tokens, never raw px
- Font sizes, padding, margin: always use fluid tokens defined in base-design.md
- Fixed px only permitted for borders, icon sizes, and border-radius
- Never invent arbitrary values — pick the nearest token from the scale

### 3. CSS formatting — single-line rules
All CSS rules on one line per selector. Media queries keep their braces on separate
lines but inner rules stay single-line. See sop_v2.md for examples.

### 4. After every CSS edit — screenshot check
Run Playwright at 375px, 768px, 1440px. Fix any overflow, squashed text,
or broken grid before marking the task done.

### 5. File structure — flat root
- index.html, contact.html, services.html sit at repo root
- All styles: assets/css/styles.css (one file, linked per page)
- All images: assets/images/
- All JS: assets/js/ (includes.js handles shared nav/footer)
- Never create duplicate draft files

### 6. Shared nav/footer — includes.js
Nav and footer injected via fetch from assets/includes/header.html and
assets/includes/footer.html. Never hardcode nav or footer in page files.

### 7. Publishing workflow
1. Edit files directly in the repo
2. Screenshot check at 375/768/1440px
3. Commit and push to main → Cloudflare Pages auto-deploys

### 8. Base design is read-only during builds
Never modify base-design.md during a client build. If something is genuinely
missing from the token scale or rules, stop and flag it with:
"BASE DESIGN GAP: [what's missing and why]. Should this be added to base-design.md?"
