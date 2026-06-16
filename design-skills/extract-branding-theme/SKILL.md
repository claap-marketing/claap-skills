---
name: extract-branding-theme
description: >
  Reverse-engineer the complete branding template from any PowerPoint (.pptx) file and output it as structured JSON,
  with extracted logo and image assets saved to disk. Use this skill whenever the user uploads or references a .pptx
  file and wants to extract its design system, branding, theme, visual identity, color palette, typography, layout grid,
  or reusable assets. Also trigger when the user says things like "extract the branding", "get the design tokens",
  "reverse-engineer the template", "what fonts/colors does this deck use", "extract the theme", "pull out the style guide",
  "analyze the design of this pptx", or "I want to replicate this deck's look". This skill is specifically for
  EXTRACTION and ANALYSIS — not for creating new decks. If the user wants to create a deck, use a deck-building skill instead.
---

# Extract Branding Theme

Reverse-engineer a .pptx file into a complete, structured JSON design system with extracted assets.

## When to use

- User uploads a .pptx and wants its design system, theme, colors, fonts, or layout extracted
- User wants to replicate a deck's visual style in another tool or format
- User wants a JSON representation of a deck's branding to feed into a builder workflow
- User says "extract", "reverse-engineer", "analyze", "pull out", "what does this deck use"

## Overview

This skill runs a 7-phase analysis pipeline using `python-pptx` and direct ZIP inspection:

1. **Theme & Colors** — theme color slots + every fill/font/border color resolved to hex, with frequency
2. **Typography** — every text run clustered into labeled styles (title, body, caption…)
3. **Layout & Spatial** — margins, grid, zones, placeholder positions in inches + px + EMU
4. **Masters & Layouts** — full inventory of slide masters, layouts, placeholders, decorative shapes
5. **Assets** — extract all images from `ppt/media/`, classify (logo/icon/photo/decorative/background), save brand assets
6. **Components** — dividers, bullet styles, table styles, CTA shapes, card patterns, chart color sequences
7. **Metadata** — author, company, transitions, custom properties

## How to execute

### Step 0 — Install dependency

```bash
pip install python-pptx Pillow --break-system-packages -q
```

### Step 1 — Locate the file

The uploaded .pptx is at `/mnt/user-data/uploads/`. List that directory to find it.

### Step 2 — Run the extraction script

```bash
python /path/to/this/skill/scripts/extract_branding.py "/mnt/user-data/uploads/<filename>.pptx"
```

The script outputs:
- `/mnt/user-data/outputs/branding_template.json` — the full JSON design system
- `/mnt/user-data/outputs/assets/` — all extracted images, with brand assets identified

If the script fails or doesn't cover an edge case, fall back to the manual extraction steps below.

### Step 3 — Review and enrich

After the script runs, open the JSON and review it. Enrich with your own analysis:

- **Role labeling**: The script clusters text styles mechanically. Review the `role` assignments — rename if a "heading" is clearly a "section_title" in context.
- **Grid detection**: If the script reports `"detected_pattern": "unknown"`, look at shape positions yourself and describe the grid (e.g., "3-column with 0.3in gutter").
- **Asset classification**: The script uses heuristics (size, position, master presence). Verify logo classifications — if there's a small image on the master that the script missed, reclassify it.
- **Gradient parsing**: python-pptx has limited gradient support. If you see `"gradients": []` but the deck clearly uses gradients, inspect the XML manually:
  ```python
  from lxml import etree
  # Access shape XML: shape._element.xml
  ```

### Step 4 — Present results

1. Use `present_files` on the JSON and key assets
2. Give the user a quick visual summary: dominant colors (with swatches if using an artifact), font stack, key layout dimensions
3. Ask if they want the JSON tweaked or want you to build a new deck using this extracted theme

## Manual fallback procedure

If the script is unavailable or fails, execute the extraction inline using this sequence. Read the full JSON schema from `references/json_schema.md` for the expected output structure.

### Phase 1: Theme Colors

```python
from pptx import Presentation
from pptx.util import Inches, Pt, Emu
from pptx.dml.color import RGBColor
from pptx.enum.dml import MSO_THEME_COLOR
import json, os, zipfile, collections

prs = Presentation(filepath)

# Theme color slots
theme_el = prs.slide_masters[0].element
ns = {'a': 'http://schemas.openxmlformats.org/drawingml/2006/main'}
clrScheme = theme_el.find('.//a:clrScheme', ns)
theme_colors = {}
for child in clrScheme:
    tag = child.tag.split('}')[-1]
    srgb = child.find('.//a:srgbClr', ns)
    sys = child.find('.//a:sysClr', ns)
    if srgb is not None:
        theme_colors[tag] = f"#{srgb.get('val')}"
    elif sys is not None:
        theme_colors[tag] = f"#{sys.get('lastClr', sys.get('val', '000000'))}"
```

Then walk every shape on every slide to collect fill colors, font colors, and border colors. Resolve theme color references using the mapping above. Track frequency with `collections.Counter`.

### Phase 2: Typography

Walk every text run. For each, extract: `font.name`, `font.size`, `font.bold`, `font.italic`, `font.color.rgb`, paragraph alignment, line spacing, space before/after. Resolve theme font references (`+mj-lt` → major font name, `+mn-lt` → minor font name) by reading:

```python
theme_xml = prs.slide_masters[0].element.find('.//a:theme', ns)
# or from the theme part directly:
theme_part = prs.slide_masters[0].element.find('.//a:majorFont', ns)
```

Cluster identical style combinations. Assign role labels based on font size ranking and placeholder type context:
- Largest size on title placeholders → `title`
- Second largest → `heading` or `subtitle`
- Most frequent mid-size → `body`
- Smallest → `caption` or `footnote`

### Phase 3: Layout & Spatial

For each slide, measure all shape bounding boxes. Derive:
- **Content margins**: smallest left, smallest top, largest (slide_width - shape.left - shape.width) for right, etc.
- **Gutters**: for shapes in the same horizontal band, measure gaps between adjacent right edges and left edges
- **Zones**: cluster shapes into header (top 15%), footer (bottom 10%), sidebar (leftmost 25% if consistently occupied), content (everything else)

### Phase 4: Masters & Layouts

```python
for master in prs.slide_masters:
    for layout in master.slide_layouts:
        # layout.name, layout.placeholders, layout.shapes
```

### Phase 5: Assets

```python
with zipfile.ZipFile(filepath, 'r') as z:
    media_files = [f for f in z.namelist() if f.startswith('ppt/media/')]
    os.makedirs('/mnt/user-data/outputs/assets', exist_ok=True)
    for mf in media_files:
        z.extract(mf, '/tmp/pptx_extract')
        # Copy to outputs, classify based on size/position/master presence
```

Classification heuristics:
- **logo**: small image (< 200x200px) present on master or layout, or appears on 50%+ of slides
- **background**: dimensions close to slide dimensions
- **icon**: small, square-ish, appears alongside text
- **decorative**: on master/layout but not logo-sized
- **photo**: large, unique to a single slide

### Phase 6: Components

Scan for recurring patterns:
- **Dividers**: thin rectangles (height < 5pt or width < 5pt) that span most of the slide width/height
- **Bullets**: read `paragraph._pPr` XML for `buChar`, `buFont`, `buSzPct`
- **Tables**: extract header row fill, cell borders, alternating row colors from `tbl` XML
- **CTA buttons**: rounded rectangles with solid fills and centered text

### Phase 7: Metadata

```python
props = prs.core_properties
# props.author, props.company, props.title, props.created, props.modified
```

### Final output

Save to `/mnt/user-data/outputs/branding_template.json` using:
```python
with open('/mnt/user-data/outputs/branding_template.json', 'w') as f:
    json.dump(result, f, indent=2, ensure_ascii=False, default=str)
```

## Reference files

- `references/json_schema.md` — Complete JSON output schema with field descriptions
- `scripts/extract_branding.py` — Automated extraction script
