---
name: design-system-extractor
description: "Extracts a full design system from a live product (website via Claude Chrome extension, or Figma file via Dev Mode MCP) and returns a drop-in design-system skill for Claude. Use when building Claude artifacts, dashboards, or mini-apps that must feel native to an existing product. Triggers on: extract design system, design system from website, design system from figma, build a design-system skill, reverse-engineer product UI."
user-invocable: true
---

# Design System Extractor

Point this skill at a live website or a Figma file, get back a drop-in design-system skill that matches the `claap-design-system` format — tokens, component recipes, motion, interaction states. Drop the output into any Claude project to produce artifacts that feel native to that product.

---

## When to use

- The user wants Claude artifacts (dashboards, story pages, mini-apps) that feel like an extension of a specific product, not just "styled with the right colors".
- A `branding-skill` is not enough: the output needs the actual component library, motion language, and interaction patterns of the product.
- No design-system skill exists yet for that product.

If a design-system skill already exists for the target product, skip this skill and load that one directly.

---

## Output contract

A single markdown file with this structure (mirroring `claap-design-system`):

```markdown
---
name: <product-slug>-design-system
description: "<Product> design system in <Tailwind/CSS/...>. Use when building Claude artifacts, HTML/React prototypes, or any <Product> UI. Covers setup boilerplate, color tokens, typography, and component recipes. Triggers on: <product> design, <product> UI, <product> artifact, <product> prototype, build <product> interface."
user-invocable: true
---

# <Product> Design System — <framework>

<One-line product posture: dark-first vs light-first, primary font, accent color, anything non-obvious.>

## ⚠️ Design Rules (read first)

<2-column table: ❌ Never / ✅ Do instead — capture the rules that are easy to get wrong. Aim for 5–10 rules, not exhaustive.>

## Setup Boilerplate

<HTML shell with tailwind.config or CSS variables. Full token scale for grays, primary/accent, semantic colors, radii, shadows, font family.>

## Typography

<Heading vs body: family, weight, size, line-height, tracking, uppercase rules.>

## Components

<Card, Button (primary / secondary / ghost), Flashcard, Stat callout, Accordion, Tab, Nav item, Modal. For each: HTML/JSX snippet with exact class names, plus hover/active/focus variants.>

## Motion

<Transitions, easing, duration, hover choreography, scroll animations. Framer Motion presets if applicable.>

## Layout

<Container widths, spacing scale, section rhythm, breakpoints.>
```

The output must be copy-paste-ready. A reader should not have to guess a value.

---

## Path A — Extract from a live website (Claude Chrome extension)

Use when: the product has a public website that represents the design system (homepage, app screenshots, or an in-app view the user can navigate to).

1. Confirm the user has the **Claude Chrome extension** installed and active.
2. Ask the user to open the target page in the active tab (typically the homepage; product app is even better if they're logged in).
3. Request this extraction:

```
Extract the full design system from this page for a reusable design-system skill.

Capture:
- Color palette: backgrounds (dark/light/neutrals with full 50→1700 scale if the product uses one), accents, text colors, gradients. Exact hex or HSL.
- Typography: heading (display, weight, tracking, uppercase rules), body, captions. Exact families, weights, sizes, line-heights.
- Component patterns: cards, buttons (primary/secondary/ghost), flashcards, stat callouts, accordions, tabs, nav items, modals. For each: class names, padding, border, radius, shadow, plus hover/active/focus variants.
- Logo: exact URL to the SVG or PNG.
- Layout: spacing scale, container widths, section rhythm, dark-first vs light-first, breakpoints.
- Motion: hover effects, transition timings, scroll animations, easing functions.
- Interaction rules: what's clickable looks clickable how, active states, disabled states.

Navigate to 2–3 additional pages (product, pricing, blog, a customer story) to confirm tokens hold and flag drift.

Return the output as a markdown file following the design-system-extractor output contract — ready to save as `<product-slug>-design-system.md` and drop into a Claude project as a skill.
```

4. Review for gaps: if a critical component is missing (e.g. the site has no visible modal), ask the user to navigate to a page where it's exposed, or note the gap explicitly in the output.
5. Save as `<product-slug>-design-system.md` in the user's preferred location.

**Fallback without the Chrome extension:** use Claude's WebFetch on 3–5 URLs (homepage, pricing, blog, product) and request the same extraction. Flag in the output that computed styles were inferred from HTML, not from rendered CSS — token accuracy will be lower.

---

## Path B — Extract from a Figma file (Dev Mode MCP)

Use when: the product has a Figma design system file with variables and styles defined (most design-system-first teams).

1. Confirm the user has the Figma **desktop app** open and the Dev Mode MCP enabled (Figma menu → Preferences → Enable Dev Mode MCP Server, default endpoint `http://127.0.0.1:3845/sse`).
2. Confirm the Figma MCP is added in Claude (Desktop or Code) and connected.
3. Ask the user to right-click the frame or component page they want to extract and choose **Copy link to selection**, then paste it.
4. Request this extraction:

```
Read this Figma frame via the Dev Mode MCP and extract the full design system:

- Variables: color tokens (with full scales), spacing tokens, radius tokens, shadow tokens, opacity tokens — exact names and values from the Figma variables panel.
- Text styles: heading display, body, captions — exact font families, weights, sizes, line-heights, letter-spacing.
- Component patterns: cards, buttons, callouts, flashcards, accordions, tabs, nav items, modals. For each: structure, variants (primary/secondary/ghost, hover/active/disabled), padding, border, radius, shadow.
- Layout grids and breakpoints.
- Effects and motion styles if defined in the file.

Return the output as a markdown file following the design-system-extractor output contract — ready to save as `<product-slug>-design-system.md` and drop into a Claude project as a skill.
```

5. If the Figma file lacks component variants or motion definitions, supplement with Path A (live site) for the missing pieces.
6. Save as `<product-slug>-design-system.md`.

---

## Path C — Combined (best quality)

For maximum fidelity, run both paths and merge:
- Figma gives exact token values (the source of truth for color, spacing, type scales).
- Live site gives component behavior, motion, and interaction states that aren't always captured in Figma.

Merge into a single skill file, noting which paths contributed which section.

---

## Validation checklist

Before handing the skill back to the user:

- [ ] Frontmatter `name` matches `<product-slug>-design-system`.
- [ ] Frontmatter `description` includes trigger phrases the user would say.
- [ ] Setup Boilerplate is copy-paste-ready (valid HTML, valid Tailwind config).
- [ ] Color scale includes all steps the product uses (don't truncate at 500; if the product uses `gray-1700`, include it).
- [ ] Component section has at least: Card, Button, Flashcard, Nav item, Stat callout. Add more if the product uses them.
- [ ] Motion section specifies duration + easing in numbers, not adjectives.
- [ ] Design Rules table exists and is specific (not generic UX truisms).
- [ ] File is under 500 lines. If longer, split by adding `references/` for rarely-used components.

---

## Examples

- `claap-design-system` — extracted from claap.io, dark-first, Inter, blue primary. See `claap-marketing/claap-skills` repo.
- `extract-branding-theme` — sibling skill that reverse-engineers `.pptx` decks into a design system (for slide generation workflows). Use this skill for mini-app / dashboard / artifact workflows; use `extract-branding-theme` for deck workflows.
