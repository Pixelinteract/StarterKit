# FreeRange Websites — Base Design System

System-wide structural rules. Copy unchanged into every new client repo.
Never edit this file during a client build — brand overrides go in brand-design.md.
Last updated: July 2026 (v1.0)

---

## Open Props — Import

Add this as the FIRST line of assets/css/styles.css on every client site:

```css
@import "https://unpkg.com/open-props";
```

---

## Semantic token block

Add this :root block to assets/css/styles.css immediately after the Open Props import.
These are the only sizing values permitted across all components.

```css
:root {
  /* Type scale */
  --text-xs:   clamp(0.75rem, 1.5vw, 0.875rem);
  --text-sm:   clamp(0.875rem, 1.8vw, 1rem);
  --text-base: clamp(1rem, 2vw, 1.125rem);
  --text-md:   clamp(1.1rem, 2.5vw, 1.35rem);
  --text-lg:   clamp(1.25rem, 3vw, 1.75rem);
  --text-xl:   clamp(1.5rem, 4vw, 2.25rem);
  --text-2xl:  clamp(2rem, 5vw, 3rem);
  --text-3xl:  clamp(2.5rem, 6vw, 4rem);

  /* Spacing scale */
  --space-1: clamp(4px, 1vw, 8px);
  --space-2: clamp(8px, 2vw, 16px);
  --space-3: clamp(16px, 3vw, 24px);
  --space-4: clamp(24px, 4vw, 40px);
  --space-5: clamp(40px, 6vw, 64px);
  --space-6: clamp(64px, 8vw, 100px);
  --space-7: clamp(80px, 10vw, 140px);
}
```

---

## Breakpoints

Only these two breakpoints are permitted. Never invent new ones.

| Name   | Value           | Use for                               |
|--------|-----------------|---------------------------------------|
| Tablet | max-width 960px | Grid reflow, sidebar stacking         |
| Mobile | max-width 640px | Nav collapse, single column, spacing  |

---

## Container

Max content width is always 1160px, centered, with fluid side padding.

```css
.container {max-width: 1160px;margin: 0 auto;padding: 0 var(--space-3);}
```

---

## Type scale — when to use what

| Token        | Use for                            |
|--------------|------------------------------------|
| --text-xs    | Labels, tags, fine print           |
| --text-sm    | Captions, secondary body           |
| --text-base  | Main body copy, list items         |
| --text-md    | Lead paragraphs, subtitles         |
| --text-lg    | H3, card headings                  |
| --text-xl    | H2, section headings               |
| --text-2xl   | H1, hero headings                  |
| --text-3xl   | Hero display / jumbo headline      |

---

## Spacing scale — when to use what

| Token     | Use for                            |
|-----------|------------------------------------|
| --space-1 | Tight gaps, icon spacing           |
| --space-2 | Inner element gaps                 |
| --space-3 | Card padding, container side pad   |
| --space-4 | Component padding                  |
| --space-5 | Section padding vertical           |
| --space-6 | Large section padding              |
| --space-7 | Hero padding vertical              |

---

## Common mistakes — catch these before finishing

| Wrong                        | Right                                   |
|------------------------------|-----------------------------------------|
| padding: 80px 20px           | padding: var(--space-6) var(--space-3)  |
| font-size: 32px              | font-size: var(--text-2xl)              |
| margin-bottom: 48px          | margin-bottom: var(--space-5)           |
| font-size: clamp(custom...)  | Use tokens from scale above only        |
| padding: 150px 0 80px 25px   | padding: var(--space-7) 0 var(--space-6) var(--space-3) |
