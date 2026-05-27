---
name: design-system-extractor
description: "Extracts a full design system from a live product (website via Claude Chrome extension, or Figma file via Dev Mode MCP) and returns a drop-in design-system skill for Claude — same shape and depth as the Claap design-system skill. Use when building Claude artifacts, dashboards, or mini-apps that must feel native to an existing product. Triggers on: extract design system, design system from website, design system from figma, build a design-system skill, reverse-engineer product UI."
user-invocable: true
---

# Design System Extractor

Point this skill at a live website or a Figma file, and it returns a drop-in `<product>-design-system` skill that matches the `claap-design-system` reference in both shape and depth — tokens, component recipes (Tailwind, copy-paste-ready), motion, interaction states.

The canonical example this skill mirrors: [`claap-design-system/SKILL.md`](https://github.com/claap-marketing/claap-skills/blob/main/claap-design-system/SKILL.md). Read it once before running — the output you generate must match that structure section-for-section.

---

## When to use

- The user wants Claude artifacts (dashboards, story pages, mini-apps) that feel like an extension of a specific product, not just "styled with the right colors".
- A `branding-skill` is not enough: the output needs the actual component library, motion, and interaction patterns.
- No design-system skill exists yet for that product.

If a design-system skill already exists for the target product, skip this skill and load that one directly.

---

## Output contract — required shape

The output is a **single markdown file** saved as `<product-slug>-design-system.md` (or packaged as a `.skill` for Claude.ai upload). Every section below is **required** unless the product genuinely does not use that component.

Use the `claap-design-system` skill as your structural reference. Match its depth — not a summary, not a placeholder. Every component must have a working code snippet with real class names, real colors, real values.

### Required sections, in order

1. **YAML frontmatter**
   ```yaml
   ---
   name: <product-slug>-design-system
   description: "<Product> design system in <Tailwind/CSS/...>. Use when building Claude artifacts, HTML/React prototypes, or any <Product> UI. Covers setup boilerplate, color tokens, typography, and all component recipes. Triggers on: <product> design, <product> UI, <product> artifact, <product> prototype, build <product> interface."
   user-invocable: true
   ---
   ```

2. **Title + one-line posture**
   `# <Product> Design System — <framework>` followed by a single line capturing dark-first vs light-first, primary font, accent color, and any non-obvious rule (e.g. "Dark-first UI. Inter font. Blue as primary. No pink in UI.").

3. **⚠️ Design Rules (read first)** — a 2-column table of `❌ Never` / `✅ Do instead` pairs. Aim for 5–10 rules capturing the things that are easy to get wrong. Follow with a one-line **Color roles** summary (e.g. "Blue = all interactive · Pink = logo only · Red = errors · Green = success").

4. **Setup Boilerplate** — a complete HTML shell with `<head>`, Tailwind CDN, Google Fonts import, and a full `tailwind.config` including:
   - `fontFamily` with system stack fallback
   - `colors` — full scale for the product's primary neutral (e.g. gray 25 → 1700), plus all accent scales the product uses (blue, pink, red, green, yellow, purple, turquoise, bordeaux, etc.) with exact hex values
   - `borderRadius` (sm / DEFAULT / md / lg / xl) with pixel values
   - `boxShadow` (modal, popin, or equivalent) with exact rgba values
   - `<style>` block with body font + scrollbar reset
   - `<body>` with the product's page background class and text color

5. **Tokens** — four sub-tables:
   - **Surfaces (dark → light)**: Role | Dark class + hex | Light class. Include page bg, card/tile, panel, hover, tooltip/dropdown, modal overlay, primary action.
   - **Text**: Role | Class. Headings, body, secondary, muted/labels, links/actions, placeholder, error / success / warning.
   - **Borders**: Role | Class. Default, dropdown/modal, hover lift, divider.
   - **Radius** — one-liner listing each radius token with its pixel value and typical use.
   - **Typography** — size table (px + Tailwind class) for 2xs / xs / sm / md / lg / xl / 2xl. Weights line listing `font-normal`, `font-medium`, `font-semibold`, `font-bold` with numeric values.

6. **Components** — every component below that the product uses, each with:
   - A short intro line if behavior is non-obvious (e.g. "Free-floating. Active = standalone dark pill. No container, no underline.")
   - One or more working HTML snippets with complete Tailwind classes
   - Variant guidance when applicable (sizes, states)

   Required component list (skip only if the product genuinely lacks it):
   - **Buttons** — primary, secondary, ghost, destructive, icon-only. Plus a sizes line.
   - **Tabs**
   - **Cards** — standard, panel/inner tile, hoverable list row
   - **Form & Inputs** — field (label above + flat input), password with eye icon, textarea. Plus a **full settings form** example showing a 2-col grid, sections, divider, and save button.
   - **Toggle Switch** — ON and OFF states. Plus a settings row (label + toggle) showing the container style.
   - **Checkbox**
   - **Badges & Dot Labels** — tag (colored bg + border), color-matrix line, dot label (for tables/lists), count pill
   - **Avatar** — image + initials. Size line.
   - **Table** — full example with header, row with hover state, dot-label cell, pagination row
   - **Grid Cards**
   - **Sidebar** — with logo, active item, inactive item, workspace/user footer. Call out the active-state rule (e.g. "Active item = background fill, no left border.")
   - **Collapsible Section**
   - **Modal**
   - **Dropdown Menu**
   - **Toast**
   - **Chat Input** (or equivalent main-action input)

7. **Logo** — exact snippet with the product's logo CDN URL plus a sizes line (e.g. `h-6` (24px) · `h-8` (32px) · `h-10` (40px)).

### Shape rules

- Code snippets must use **complete Tailwind classes** — no placeholder like `/* classes here */` or `bg-<primary>`. Write `bg-blue-400` with the product's actual hex mapped in the config.
- Tables must use pipe-syntax Markdown, not HTML.
- Use `---` dividers between major sections (Design Rules, Setup, Tokens, Components, Logo).
- File target length: ~500–600 lines. Longer is fine if the product has more components. Shorter means something is missing.

---

## Path A — Extract from a live website (Claude Chrome extension)

Use when: the product has a public website or in-app view the user can navigate to.

1. Confirm the user has the **Claude Chrome extension** installed and active.
2. Ask the user to open the target page in the active tab. Best results: the logged-in product app (real components in real states). Second best: homepage. Third: pricing or dashboard marketing pages.
3. Request this extraction (pass the full prompt; do not summarize):

```
Extract the full design system from this page and every other page I navigate to,
for a reusable design-system skill that matches the claap-design-system structure
at https://github.com/claap-marketing/claap-skills/blob/main/claap-design-system/SKILL.md.

Capture with exact values (hex, px, class names — no placeholders):

1. Color palette
   - Full scale for the primary neutral (e.g. gray 25 through gray-1700 or equivalent).
     If the product uses a 17-step scale, capture all 17 steps with hex.
   - Every accent scale in use (blue, pink, red, green, yellow, purple, turquoise, bordeaux, etc.)
     with 3–5 steps each.
   - Gradients, overlays, backdrop filters with exact rgba.

2. Typography
   - Font families (heading / body / mono) with full Google Fonts import URL.
   - Size scale in px + Tailwind class names: 2xs (11) / xs (12) / sm (13) / md (15) / lg (18) / xl (20) / 2xl (24) — map to what the product actually uses.
   - Weights (400 / 500 / 600 / 700) with sample usage.
   - Letter-spacing and uppercase rules if used (labels in tables, section headers).

3. Components — for each: a working HTML snippet with complete Tailwind classes, plus variant notes.
   - Buttons: primary, secondary, ghost, destructive, icon-only, plus a sizes line.
   - Tabs: capture whether they are container-based or free-floating, active state style.
   - Cards: standard, panel/inner, hoverable list row.
   - Forms: label + input field, password with eye, textarea, plus a full settings form example.
   - Toggle switch: ON / OFF plus settings row container.
   - Checkbox.
   - Badges and dot labels: tag with colored bg+border, dot label for tables, count pill.
   - Avatar: image and initials, size line.
   - Table: header, row with hover, cell with dot label, pagination.
   - Grid cards.
   - Sidebar: logo, active item, inactive item, workspace footer. Call out the active-state rule.
   - Collapsible section.
   - Modal, dropdown menu, toast, chat input.

4. Motion
   - Transition durations (ms) and easing (ease-out / ease-in-out / bezier).
   - Hover choreography (color lift, border change, shadow).
   - Scroll or reveal animations if used.

5. Interaction rules
   - What's clickable looks clickable how.
   - Active / disabled / focus states.
   - Blue border on hover is a common anti-pattern — flag if the product uses a different pattern (e.g. gray lift).

6. Logo — exact URL to the SVG on the product CDN, plus the 2–3 heights the brand uses.

Navigate to 3+ additional pages (product app, pricing, a detail page, a settings page)
to confirm tokens hold and flag drift. If the product has a logged-in view, prefer it.

Return the output as a single markdown file following the design-system-extractor
output contract — ready to save as `<product-slug>-design-system.md`. Do not summarize.
Do not use placeholders. Every class must be a real, working Tailwind class.
```

4. Review for gaps. If a critical component is missing (e.g. no visible modal), ask the user to navigate to a page where it's exposed, or note the gap explicitly in the output.
5. Save as `<product-slug>-design-system.md`.

**Fallback without the Chrome extension:** use Claude's WebFetch on 5+ URLs (homepage, pricing, blog, product, a detail page) with the same prompt. Flag in the output that computed styles were inferred from HTML, so token accuracy will be lower than a rendered-DOM extraction.

---

## Path B — Extract from a Figma file (Dev Mode MCP)

Use when: the product has a Figma design system with variables and component styles defined (most design-system-first teams).

1. Confirm the user has the Figma **desktop app** open and the Dev Mode MCP enabled (Figma menu → Preferences → Enable Dev Mode MCP Server, default endpoint `http://127.0.0.1:3845/sse`).
2. Confirm the Figma MCP is added in Claude (Desktop or Code) and connected.
3. Ask the user to right-click the design-system page (or the main component frame) and **Copy link to selection**, then paste it.
4. Request this extraction:

```
Read this Figma frame via the Dev Mode MCP and extract the full design system
for a reusable design-system skill matching the claap-design-system structure at
https://github.com/claap-marketing/claap-skills/blob/main/claap-design-system/SKILL.md.

Use the Figma variables panel as the source of truth for tokens. Capture:

1. Variables — exact names and values from the Figma variables panel:
   - Color tokens (full scales, all accents)
   - Spacing tokens
   - Radius tokens
   - Shadow tokens
   - Opacity tokens

2. Text styles — every style defined in the file with exact font family, weight,
   size (px), line-height, letter-spacing.

3. Component patterns — every component set in the library. For each, capture:
   - Structure (children, auto-layout rules)
   - Variants (primary/secondary/ghost, default/hover/active/disabled)
   - Padding, border, radius, shadow

4. Layout grids and breakpoints — column counts, gutters, max widths.

5. Effects and motion styles if defined in the file.

Generate a single markdown file matching the design-system-extractor output contract.
Translate Figma components into working Tailwind HTML snippets. Every class must be
a real Tailwind class — no placeholders.

If a component defined in the contract is not in the Figma file, note the gap
explicitly and suggest running Path A (live site extraction) to fill it.
```

5. If the Figma file lacks component variants, motion definitions, or interaction states, supplement with Path A to fill the gaps.
6. Save as `<product-slug>-design-system.md`.

---

## Path C — Combined (best quality)

For maximum fidelity, run both paths and merge:
- **Figma gives exact token values** (the source of truth for color, spacing, type scales).
- **Live site gives component behavior, motion, and interaction states** that aren't always captured in Figma.

Merge into a single skill file. For each component, prefer the live-site snippet (it's what ships) but use Figma values for tokens referenced inside.

---

## Validation checklist

Before handing the skill back to the user, confirm:

- [ ] Frontmatter `name` is `<product-slug>-design-system`.
- [ ] Frontmatter `description` includes 4–6 trigger phrases the user would actually say.
- [ ] Title has the framework name appended (e.g. "— Tailwind").
- [ ] One-line posture under the title captures dark-first/light-first + primary font + primary accent + one non-obvious rule.
- [ ] Design Rules table has 5+ `❌ Never / ✅ Do instead` rows.
- [ ] Setup Boilerplate is a complete HTML file that renders as-is — CDN, fonts import, full tailwind.config, body classes.
- [ ] Color scale includes every step the product uses (don't truncate at 500 if the product uses 1500+).
- [ ] Tokens section has Surfaces + Text + Borders + Radius + Typography tables.
- [ ] Every required component has a code snippet. Buttons includes primary + secondary + ghost + destructive + icon + sizes line.
- [ ] Full settings form example exists in the Forms section.
- [ ] Table example shows header, hover state, dot-label cell, and pagination.
- [ ] Sidebar snippet shows logo, active item, inactive item, and footer.
- [ ] Logo block has the exact CDN URL and a sizes line.
- [ ] No placeholders like `<primary>` or `/* ... */` in code snippets. Every class is real Tailwind.
- [ ] File length is 500–600+ lines (shorter = missing content).

---

## Examples

- **Reference output:** [`claap-design-system/SKILL.md`](https://github.com/claap-marketing/claap-skills/blob/main/claap-design-system/SKILL.md) — what your output should look like in shape, depth, and format.
- **Sibling skill:** [`extract-branding-theme`](https://github.com/claap-marketing/claap-skills/blob/main/extract-branding-theme.skill) — reverse-engineers a `.pptx` deck into a design system for slide-generation workflows. Use this skill (`design-system-extractor`) for mini-app / dashboard / artifact workflows; use `extract-branding-theme` for deck workflows.
