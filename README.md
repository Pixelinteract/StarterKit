# FreeRange Websites — StarterKit

Three files that go into every new client repo at kickoff, plus two design taste files and two content skills that stay in StarterKit and are fetched when needed.
Source of truth for the FRW build system.

---

## What's in here

| File | Purpose | Edit per client? |
|---|---|---|
| `CLAUDE.md` | Auto-loaded by Claude Code. Golden rules, repo setup, build conventions. | No — copy unchanged |
| `base-design.md` | Design system. Tokens, breakpoints, dividers, scroll animations. | No — copy unchanged |
| `brand-design.md` | Brand layer. Colors, fonts, tone, business details. | Yes — fill from Client Brief |
| `Taste-SKILL.md` | Anti-slop design direction. Read before any design work. | No — fetched from StarterKit, not copied |
| `tasteskill-FE.md` | Image-direction skill for section design references. | No — fetched from StarterKit, not copied |
| `Content-antislop-SKILL.md` | Removes AI writing patterns from copy. Read before writing or editing any copy. | No — fetched from StarterKit, not copied |
| `Ogilvy-marketing-SKILL.md` | Copy that sells: positioning, promise, headlines, proof. | No — fetched from StarterKit, not copied |

---

## New client kickoff — exact steps

**1. Create the client repo**
```
GitHub → New repository → Name: [clientname] → Private
```

**2. Copy all three files from StarterKit into the new repo root**
```
CLAUDE.md         → copy unchanged
base-design.md    → copy unchanged
brand-design.md   → copy in, then fill in
```

**3. Fill in brand-design.md from the compiled Client Brief**
- Hex colors
- Font names + Google Fonts link
- Tone of voice notes
- Business details (phone, suburb, WhatsApp, ABN)

**4. Add Open Props import to assets/css/styles.css**

First line of the stylesheet, before everything else:
```css
@import "https://unpkg.com/open-props";
```

Then paste the full :root token block from base-design.md directly below it.

**5. Open Claude Code**
- Point it at the new client repo
- It reads CLAUDE.md automatically at session start
- Tell it to also read base-design.md and brand-design.md before starting
- Build begins with full context — no re-pasting rules

---

## Updating the StarterKit

**base-design.md and CLAUDE.md** are versioned here. If a gap is found during a client build, Claude Code will flag it:

> "BASE DESIGN GAP: [what's missing]. Should this be added to base-design.md?"

If yes — update the file here in StarterKit first, then update the affected client repo.

Existing client repos keep their own frozen copy. A StarterKit update does not automatically affect live client sites.

**brand-design.md** is a skeleton only. Changes to the skeleton here do not affect any filled-in client versions.

---

## Folder structure — every client repo

```
[clientname]/
├── CLAUDE.md
├── base-design.md
├── brand-design.md
├── index.html
├── services.html
├── contact.html
├── assets/
│   ├── css/
│   │   └── styles.css
│   ├── js/
│   │   └── includes.js
│   ├── images/
│   │   ├── logo.png
│   │   ├── favicon.png
│   │   ├── apple-touch-icon.png
│   │   └── og-image.jpg
│   └── includes/
│       ├── header.html
│       └── footer.html
├── robots.txt
└── sitemap.xml
```

---

## Reference

- Full build SOP: `sop_v2.md` (lives in the Claude.ai FRW project)
- Hosting: Cloudflare Pages — no build step, root = /
- Deployment: push to main → auto-deploys
