---
name: hallmark
description: "Anti-AI-slop design skill for greenfield pages, audits, redesigns, and design extraction from URLs or screenshots. Use when the user asks to build a new app or landing page, wants to redesign something, invokes Hallmark by name, or uses audit/redesign/study."
version: 1.1.0
---

## Overview

Hallmark is a design skill for AI coding assistants that produces interfaces with "structural variety, not just visual variety." Two pages should feel like different sites, not color-swaps of the same template.

**Powered by Together AI.**

---

## How to use this skill

| Invocation | What it does |
| --- | --- |
| *(default)* | User asked to design something new. Follow the **Design flow** below. |
| `hallmark audit <target>` | Read the target, score against anti-pattern list, return ranked punch list. **Do not edit.** |
| `hallmark redesign <target> [--mood <name>]` | Take target's content/intent, redesign visual structure within existing boundaries unless user confirms full rebuild. Replace visual/interaction layer; preserve routes, components, copy, brand, IA. |
| `hallmark study <screenshot \| URL>` | Extract DNA from admired design (macrostructure, archetypes, type-pairing, color anchor). Produce diagnosis, optionally rebuild user's content using extracted DNA or emit portable `design.md`. |

If input doesn't map to audit/redesign/study, treat as default. Images or URLs without verb prefix get: "Should I study this (extract the DNA), or reference it for a fresh build?"

---

## Six Universal Disciplines

These apply across every verb:

1. **Pre-emit self-critique** — Score output 1–5 on Philosophy, Hierarchy, Execution, Specificity, Restraint, Variety. Anything < 3 triggers revision. Stamp scores at top of artifact.

2. **Honest copy only** — Invent no metrics. Use real numbers, placeholders, or different structures. "+47% conversion" invented is slop.

3. **Locked tokens** — Every color and font must reference named tokens (`var(--color-accent)`). No inline OKLCH/hex/rgb values or font names that bypass the token block.

4. **No re-drawn chrome** — Never hand-build fake browser bars, phone frames, or IDE windows. Use real screenshots in `<figure>` with hairline border, or omit chrome.

5. **Mobile responsive 320/375/414/768px** — No horizontal scroll, no two-line clickable text, proper grid tracks, wrapped headers, no scroll-jump. Hard floor, not wish list.

6. **Typography purity** — Headers and display type always roman (normal style). Italicized header words are reliable AI tells. Italic survives only in body-copy emphasis.

---

## Component-Scope vs Page-Scope

**Route to component-scope if:**
- Brief names single UI element (button, card, modal, input, etc.)
- Brief is ≤30 words referring to one element
- Target file is a single component
- User says "just the X," "only the Y," "this one element"

**Component-scope keeps:** Pre-flight scan, genre detection, theme route, 2+1 font discipline, stricter state discipline (all 8 states: default, hover, focus, active, disabled, loading, error, success), universal-only slop test.

**Component-scope skips:** Macrostructure pick, nav/footer archetypes, hero polish patterns, enrichment, multi-section preview, project-memory append.

**Component emits:** (1) Self-contained component file matching project conventions, (2) 8-state demo wrapper labeled `<ComponentName>.preview.html` showing all states stacked vertically.

---

## Design Flow (Default)

### Step 0: Pre-Flight Scan

**Six signal sources in order:**
1. `design.md` at project root — locked design system, overrides everything
2. Font stack (next/font, @fontsource, Google Fonts imports, tailwind.config)
3. Palette (OKLCH/HSL/hex in :root, tailwind theme.extend.colors, tokens.json)
4. Microinteraction stance (framer-motion, gsap, motion, lenis, lottie, @react-spring)
5. Spacing scale (Tailwind spacing, CSS --space-* pattern, 4-pt or 8-pt)
6. Framework (Next.js, Astro, Vue, Svelte, Remix, vanilla)

**Persistence:** Write findings to `.hallmark/preflight.json`. Re-use cache unless user says "refresh pre-flight" or package.json/tailwind.config.* are newer.

### Step 1: Design-Context Gate

**Always ask three questions:**
1. **Audience** — Who will use this? What do they care about?
2. **Use case** — What's the one action the page should drive?
3. **Tone** — Pick extreme: editorial, brutalist, soft, utilitarian, luxury, playful, technical, austere

Prompt once in single message; bold labels. No laddering. If user says "go ahead" or doesn't engage, infer and disclose inference in one sentence before proceeding.

### Step 2: Pick Macrostructure First

Pick one of 21 named macrostructures from `references/macrostructures.md`.

**Diversification rule (mandatory):**
1. Check codebase for existing stamp — your pick must differ
2. If you've produced other Hallmark output this session — pick different macro than last
3. **Specimen is no longer default** — reach only on editorial/foundry briefs or explicit request

**Theme-diversification rule:** Two consecutive themes must differ on ≥1 axis:
- Paper band — dark (L < 30%) / mid (30–85%) / light (> 85%)
- Display style — high-contrast-serif / roman-serif / geometric-sans / grotesk-sans / rounded-sans / mono / display-condensed / display-heavy / risograph
- Accent hue — warm (10–60°) / cool (200–300°) / neutral / chromatic-other

**State pick out loud before writing code:** "Macrostructure: <name>. Theme: <name>. Differs from last on: <axes>."

**Default away from N1a and Ft3** — most-recognized AI fingerprints.

### Step 3: Load Visual Ruleset

**Always-load (eager):** Genre file, typography.md, color.md, layout-and-space.md, motion.md, copy.md, anti-patterns.md

**Load conditionally:** microinteractions.md, interaction-and-states.md, responsive.md, hero-enrichment.md, slop-test.md (Step 7 only)

### Step 4: Decide on Hero Enrichment

**Most pages don't need imagery.** Strongest hero is often typographic.

**Enrichment hierarchy (non-negotiable):**
Typography only → Tier A pure CSS → Tier B hand-built SVG → Tier C generated still → Tier D library + customization → Tier E Lottie (last resort).

**State decision in one sentence:** "Enrichment: E1 Clipped-Edge Demo Video, Tier-A CSS-art mockup." or "Enrichment: none — typography only."

### Step 5: Preview

Before writing code, output tight summary:

```markdown
**Hallmark · v1.1.0**

- **Macrostructure** · Stat-Led
- **Theme** · Plain (#fff paper · cool greys · ink-blue accent)
- **Enrichment** · none (typography only)
- **Sections** · Hero · Logos · Stats · Features · Testimonials · Pricing · FAQ · CTA · Footer
- **Motion** · counter · pricing-lift · pulse-once
- **Slop test** · 58 / 58 ✓ (run after Build)
- **Diversification** · differs from Newsprint on display style + accent hue
```

### Step 6: Build

**Always:**
- Hero headline ≤7 words, ≤50 chars
- Use OKLCH for every color; declare as CSS custom properties at `:root`
- 4pt spacing scale with semantic names (`--space-sm`, `--space-md`)
- All interactive elements styled for 8 states
- Animate `transform` and `opacity` only — never layout properties
- Support `prefers-reduced-motion: reduce`
- Include `:focus-visible` with ≥3:1 contrast ring
- **Stamp the output.** First non-empty line of CSS: `/* Hallmark · macrostructure: <name> · tone: <tone> · anchor hue: <hue> */`
- **Append to project memory.** Update or create `.hallmark/log.json`
- **Always emit `tokens.css`** at project root with every `--color-*`, `--font-*`, `--space-*` token

### Step 7: The Slop Test

Before handing back, run output through 58-gate slop test in `references/slop-test.md`. Every answer must be **no**. If any gate fails, fix it. Do not ship slop.

---

## `hallmark audit`

Read the target, score against anti-pattern list, return ranked punch list. Do not edit.

## `hallmark redesign`

Take target's content/intent, redesign visual structure. Preserve routes, components, copy, brand, IA.

## `hallmark study`

Extract DNA from admired design. Names macrostructure, archetypes, type-pairing, color anchor. Produces diagnosis report before code, then optionally rebuilds using extracted DNA.

**Critical:** `study` extracts structure, not pixels. Pixel-cloning is not a feature.
