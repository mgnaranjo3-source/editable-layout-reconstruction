# Reconstruction QA Checklist

## Completeness
- [ ] Every visible text block is a `text` element
- [ ] Every distinct shape behind or around content is its own element
- [ ] Nothing was invented that isn't in the source

## Text accuracy
- [ ] Characters match exactly, including accents, punctuation and symbols
- [ ] Style changes (size, weight, color) start a new element
- [ ] Font category and weight are plausible
- [ ] Alignment matches (centered text is `center`, not left with a guessed x)

## Geometry
- [ ] Boxes are tight: no large empty margins, no clipped letters
- [ ] Centered elements are symmetric around the canvas center (x + w/2 ≈ canvas width / 2)
- [ ] Values are whole pixels in the source image's scale

## Layers and groups
- [ ] Labels sit above their buttons; titles above their bands
- [ ] Buttons, headers, cards and multi-shape illustrations are grouped

## Visual check
- [ ] Re-render the reconstruction and compare side by side with the source
- [ ] Colors match within a small tolerance

## Common errors

| Error | Example | Fix |
|---|---|---|
| Text baked into a shape | Button exported as one image with the label | Split into `shape` + `text`, group them |
| Merged text blocks | Date and location in one element though sizes differ | One element per style |
| Dropped accents | "llevate" instead of "llévate" | Transcribe character by character |
| Wrong layer order | Button drawn over its own label | Raise the label's `z` |
| Loose boxes | Title box spans the whole canvas width | Fit the box to the glyphs |
| Guessed text | Unreadable footer filled in from memory | Use `text_uncertain: true` |

## Verification of the example
For [`examples/flyer-layout.json`](examples/flyer-layout.json), every centered element satisfies x + w/2 within 1 px of 400 (the canvas center), and every label has a higher `z` than its shape.
