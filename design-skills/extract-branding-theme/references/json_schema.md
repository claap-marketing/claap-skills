# JSON Output Schema — Branding Template

This is the complete schema for the `branding_template.json` output. Every color is hex `#RRGGBB`. Every dimension includes `inches`, `px` (at 96 DPI), and `emu` where applicable.

## Top-level structure

```json
{
  "meta": { ... },
  "color_palette": { ... },
  "typography": { ... },
  "layouts": { ... },
  "components": { ... },
  "assets": [ ... ],
  "slide_overrides": [ ... ]
}
```

---

## `meta`

Presentation-level metadata.

| Field | Type | Description |
|---|---|---|
| `source_filename` | string | Original filename of the .pptx |
| `slide_count` | int | Total number of slides |
| `slide_width` | dim | Slide width (`inches`, `px`, `emu`) |
| `slide_height` | dim | Slide height |
| `author` | string | Author from core properties |
| `company` | string | Company name (if set in extended properties) |
| `title` | string | Presentation title |
| `subject` | string | Subject |
| `keywords` | string | Keywords |
| `created` | string | ISO datetime of creation |
| `modified` | string | ISO datetime of last modification |
| `category` | string | Category |

---

## `color_palette`

### `theme_colors`

The 12 theme color slots resolved to hex.

| Key | Description |
|---|---|
| `dk1` | Dark 1 (usually black) |
| `dk2` | Dark 2 |
| `lt1` | Light 1 (usually white) |
| `lt2` | Light 2 |
| `accent1`–`accent6` | Accent colors 1 through 6 |
| `hlink` | Hyperlink color |
| `folHlink` | Followed hyperlink color |

### `dominant_colors`

Array of all unique colors found in the deck, sorted by frequency.

| Field | Type | Description |
|---|---|---|
| `hex` | string | `#RRGGBB` |
| `frequency` | int | Number of occurrences across all shapes |
| `usage` | string | Pipe-separated: `fill`, `text`, `border` |

### `gradients`

Array of gradient fills found in the deck.

| Field | Type | Description |
|---|---|---|
| `type` | string | `linear` or `radial` |
| `angle` | float | Angle in degrees (for linear) |
| `stops` | array | `[{ "hex": "#...", "position": 0.0–1.0 }]` |
| `used_on` | string | Where the gradient was found |

---

## `typography`

### `theme_fonts`

| Field | Description |
|---|---|
| `major` | Heading font family (resolved from `+mj-lt`) |
| `minor` | Body font family (resolved from `+mn-lt`) |

### `text_styles`

Array of distinct text style clusters, sorted by frequency.

| Field | Type | Description |
|---|---|---|
| `role` | string | Assigned role: `title`, `heading`, `subheading`, `body`, `body_bold`, `caption`, `footnote`, or custom |
| `font_name` | string | Resolved font family name |
| `font_size_pt` | float | Font size in points |
| `bold` | bool | Bold weight |
| `italic` | bool | Italic style |
| `underline` | bool | Underline |
| `color` | string | Hex color |
| `alignment` | string | `LEFT`, `CENTER`, `RIGHT`, `JUSTIFY`, or null |
| `line_spacing_pt` | float | Line spacing in points (if set) |
| `space_before_pt` | float | Space before paragraph in points |
| `space_after_pt` | float | Space after paragraph in points |
| `frequency` | int | How many text runs match this style |
| `example_text` | string | Sample text from the deck (max 80 chars) |

---

## `layouts`

### `content_margins`

Derived from the closest shape edges to each slide edge.

| Field | Type |
|---|---|
| `left` | dim |
| `top` | dim |
| `right` | dim |
| `bottom` | dim |

### `grid`

| Field | Type | Description |
|---|---|---|
| `columns` | int | Detected column count (0 = unknown) |
| `gutter` | dim | Average gap between adjacent shapes |
| `detected_pattern` | string | Description of the grid pattern |

### `zones`

Array of detected layout zones.

| Field | Type | Description |
|---|---|---|
| `name` | string | `header`, `content`, `sidebar`, `footer` |
| `left`, `top`, `width`, `height` | dim | Zone boundaries |

### `slide_layouts`

Array of all slide layouts from all masters.

| Field | Type | Description |
|---|---|---|
| `name` | string | Layout name (e.g., "Title Slide", "Two Content") |
| `master_name` | string | Parent master name |
| `background` | string | Hex color, `gradient`, `image`, or `inherited` |
| `placeholders` | array | See below |
| `decorative_shapes` | array | See below |

#### Placeholder object

| Field | Type | Description |
|---|---|---|
| `type` | string | `TITLE`, `BODY`, `SUBTITLE`, `PICTURE`, `SLIDE_NUMBER`, `FOOTER`, `DATE` |
| `idx` | int | Placeholder index |
| `left`, `top`, `width`, `height` | dim | Position and size |

#### Decorative shape object

| Field | Type | Description |
|---|---|---|
| `type` | string | Shape type (`rectangle`, `line`, `freeform`, `image`) |
| `fill` | string | Fill color hex or null |
| `position` | object | `{ left_in, top_in, width_in, height_in }` |
| `notes` | string | Additional notes |

---

## `components`

### `dividers`

| Field | Type | Description |
|---|---|---|
| `orientation` | string | `horizontal` or `vertical` |
| `color` | string | Hex color |
| `thickness_pt` | float | Line thickness in points |
| `position_pattern` | string | Description of typical position |

### `bullets`

| Field | Type | Description |
|---|---|---|
| `character` | string | Bullet character (e.g., `•`, `–`, `▸`) |
| `color` | string | Bullet color hex |
| `size_pt` | float | Bullet size |
| `indent_inches` | float | Indentation level |

### `table_styles`

| Field | Type | Description |
|---|---|---|
| `header_fill` | string | Header row background hex |
| `header_text_color` | string | Header text color |
| `row_fill_1` | string | Odd row fill |
| `row_fill_2` | string | Even row fill |
| `border_color` | string | Cell border color |
| `border_width_pt` | float | Border thickness |

### `buttons_ctas`

| Field | Type | Description |
|---|---|---|
| `fill` | string | Button background hex |
| `text_color` | string | Button text color |
| `border_radius_pt` | float | Corner radius |
| `font` | string | Font family |
| `font_size_pt` | float | Font size |

### `chart_color_sequence`

Array of hex strings representing the color order used in charts.

---

## `assets`

Array of all extracted media files.

| Field | Type | Description |
|---|---|---|
| `filename` | string | Original filename in ppt/media/ |
| `format` | string | `png`, `jpg`, `svg`, `emf`, `wmf`, etc. |
| `dimensions_px` | object | `{ width, height }` |
| `file_size_bytes` | int | File size |
| `classification` | string | `logo`, `icon`, `photo`, `illustration`, `decorative`, `background`, `vector_graphic` |
| `found_on_slides` | array[int] | Slide numbers where this image appears |
| `found_on_master_or_layout` | string | Master/layout name if applicable |
| `extracted_to` | string | Absolute path where the file was saved |
| `is_brand_asset` | bool | Whether this is likely reusable (logo, icon, vector) |

---

## `slide_overrides`

Array of slides that deviate from the default background.

| Field | Type | Description |
|---|---|---|
| `slide_number` | int | 1-indexed slide number |
| `has_custom_background` | bool | Whether background overrides the layout |
| `background_detail` | string | Hex color, `gradient`, or `image` |
| `notes` | string | Additional context |

---

## Dimension (`dim`) type

Used throughout the schema for spatial values:

```json
{
  "inches": 1.5,
  "px": 144.0,
  "emu": 1371600
}
```
