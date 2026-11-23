# Layout JSON Schema

See the full example in [`examples/flyer-layout.json`](examples/flyer-layout.json).

## Top level

| Field | Type | Description |
|---|---|---|
| `canvas` | object | `width`, `height`, `units` ("px"), `origin` ("top-left"), `background` (hex) |
| `source` | string | Path to the flat input image |
| `elements` | array | All editable elements (below) |
| `groups` | array | `{ id, members[] }` for elements that move together |
| `palette` | array | Distinct hex colors used in the design |

## Element (all types)

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | string | yes | Unique, descriptive (`cta-label`, not `text7`) |
| `type` | `text` · `shape` · `image` · `icon` | yes | Element type |
| `bbox` | `{x, y, w, h}` | yes | Whole pixels, top-left origin |
| `z` | integer | yes | Layer order; higher is on top |
| `group` | string | no | Group id this element belongs to |
| `rotation` | number | no | Degrees clockwise; default 0 |
| `opacity` | number 0–1 | no | Default 1 |

## Text elements

| Field | Type | Description |
|---|---|---|
| `text` | string | Exact transcription |
| `font_family` | string | Closest family (e.g. "Arial") |
| `font_category` | string | serif · sans-serif · script · monospace · display |
| `font_size` | number | px |
| `font_weight` | number | 400 regular, 700 bold |
| `color` | hex | Text fill |
| `align` | left · center · right | Horizontal alignment |
| `role` | string | headline · subheadline · body · body-emphasis · button-label · caption |
| `language` | string | ISO code, or `en+es` for mixed |
| `text_uncertain` | boolean | True if any characters could not be read |

## Shape elements

| Field | Type | Description |
|---|---|---|
| `shape` | rectangle · rounded-rectangle · ellipse · line · path | Geometry |
| `fill` | hex or gradient object | Fill color |
| `corner_radius` | number | For rounded rectangles |
| `stroke` | `{color, width}` | Optional outline |

## Validation rules
1. Every `id` is unique.
2. Every `bbox` lies at least partly inside the canvas.
3. Every group member exists in `elements`.
4. A text element inside a shape (same group) has a higher `z` than the shape.
5. Colors are 6-digit hex.
