# Editable Layout Reconstruction

Guidelines and a worked example for **editable-layout reconstruction**: turning a flat design image (flyer, poster, slide, social graphic) into a structured, editable layout where every text block, shape and image is a separate object with its position, size, style and layer order.

This kind of annotation is used to train and evaluate AI models that convert images into editable designs.

> The flyer in this repo is a **synthetic** design created for demonstration.

## Worked example

| Step | File |
|---|---|
| 1. Flat input image | [`examples/flyer-flat.png`](examples/flyer-flat.png) |
| 2. Structured annotation (every element, bbox, style, z-order, groups) | [`examples/flyer-layout.json`](examples/flyer-layout.json) |
| 3. Editable reconstruction built from the annotation | [`examples/flyer-reconstructed.svg`](examples/flyer-reconstructed.svg) |
| 4. Side-by-side check of input vs reconstruction | [`examples/comparison.png`](examples/comparison.png) |

## Documentation

| File | Purpose |
|---|---|
| [`guideline.md`](guideline.md) | Annotator guideline: element types, bounding boxes, text attributes, grouping, layer order, edge cases |
| [`schema.md`](schema.md) | Field-by-field description of the layout JSON |
| [`qa-checklist.md`](qa-checklist.md) | Reviewer checklist and common reconstruction errors |

## What a good reconstruction gets right
1. **Every visible element is its own object.** No text baked into a background shape.
2. **Text is real text.** Exact characters (including accents and symbols), font category, size, weight, color, alignment.
3. **Boxes are tight and consistent.** Same coordinate system, same rounding, no overlaps that don't exist in the original.
4. **Layer order is correct.** A button label sits above its button; a header band sits below its title.
5. **Logical groups are kept.** A button and its label move together; a header stays one unit.
6. **It matches when re-rendered.** The reconstruction is compared against the original side by side.

## License
MIT
