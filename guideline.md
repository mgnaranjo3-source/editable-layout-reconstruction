# Editable Layout Reconstruction — Annotator Guideline (v1.0)

## 1. Task
You receive a **flat image** of a design. Recreate it as a list of **editable elements** so a designer could open it and change any word, color or position without redrawing anything.

## 2. Coordinate system
- Origin at the **top-left** corner of the canvas; x grows right, y grows down.
- Units: **pixels of the source image**. Do not rescale.
- Round all values to **whole pixels**.
- A bounding box is `{x, y, w, h}`: the top-left corner plus width and height.

## 3. Element types

| Type | Use for | Required attributes |
|---|---|---|
| `text` | Any readable characters | text, font_category, font_size, font_weight, color, align |
| `shape` | Solid or gradient geometric forms: rectangles, rounded rectangles, ellipses, lines | shape, fill (and corner_radius if rounded) |
| `image` | Photos, complex illustrations, logos that can't be rebuilt from shapes | crop reference or description |
| `icon` | Small symbolic graphics (arrows, social icons) | name/description |

**Rule:** if it can be typed, it is `text`. Never mark readable words as `image`.

## 4. Text rules
1. **Transcribe exactly**: same capitalization, punctuation, accents (é, ñ, ¿, ¡) and symbols (·, –, &).
2. **One element per text block.** Lines that share a style and move together are one block; a line with a different style (size, color, weight) is its own block.
3. **Bounding box** wraps the visible glyphs tightly, from the top of the tallest letter to the bottom of the lowest descender.
4. **Font**: if the exact font is unknown, record the **category** (serif, sans-serif, script, monospace, display) and the closest common family.
5. **Size**: estimate from the cap height and x-height; record in px.
6. **Color**: sample the solid fill of the letters, not anti-aliased edges. Use hex (`#1F4E5F`).
7. **Language**: tag non-English blocks (`"language": "es"`); mixed blocks are `"en+es"`.

## 5. Shapes
- Use the simplest shape that matches: rectangle → rounded rectangle → ellipse → path.
- A shape behind text is a separate element from the text.
- Record `corner_radius` for rounded corners (a pill button's radius is half its height).

## 6. Layer order (z)
- `z = 1` is the bottom-most element above the canvas background.
- Anything drawn on top gets a higher `z`.
- Text on a button or band must have a higher `z` than the button or band.

## 7. Groups
Group elements that belong together and should move as one unit:
- header (band + title + subtitle)
- button (shape + label)
- illustration made of several shapes
- a card (background + its text)

## 8. Edge cases

| Case | Decision |
|---|---|
| Text on a photo | Two elements: `image` (photo) + `text` above it |
| Curved or rotated text | `text` with `rotation` in degrees; note `"path": "curved"` if it follows a curve |
| Gradient fill | Record start and end colors and angle |
| Drop shadow | Record as a style on the element, not a separate shape |
| Partially cropped element at the canvas edge | Keep the full shape; the canvas crops it |
| Watermark or faint background text | Annotate it as `text` with its real opacity |
| Logo with words | `image` (logo) unless it is plain typed text |

## 9. Do not
- Merge separate text blocks into one paragraph.
- Leave text inside a flattened image.
- Guess words you cannot read; mark `"text_uncertain": true` and describe.
- Invent elements that aren't visible.
