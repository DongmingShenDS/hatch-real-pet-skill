# QA Rubric

Do not accept an atlas until all checks pass.

## Geometry

- Exact `1536x1872` dimensions.
- 8 columns x 9 rows.
- Each frame fits inside its `192x208` cell.
- Unused cells are transparent.
- `qa/review.json` has no errors.
- `frames/frames-manifest.json` records component extraction for production rows, unless slot extraction was intentionally accepted after visual inspection.

## Character Consistency

- Same silhouette and proportions across every row.
- Same real-pet identity: species, breed/body type, face, markings, texture, and expression language.
- Same material, realistic colors, lighting consistency, accessory placement, and prop design.
- No frame introduces a new unintended character or object.

## Real Pet Style

- Art reads as a realistic reference-faithful pet cutout, not pixel art, a cartoon mascot, anime character, toy-like 3D render, vector logo, sticker, generic stock animal, or glossy app icon.
- Full-body silhouette is compact enough to read inside a `192x208` cell.
- Edges are crisp enough for chroma-key extraction without artificial dark outline overlays, stepped pixel edges, soft glows, soft shadows, or loose flyaway wisps.
- Realistic markings, fur/skin/feather/material texture, colors, and accessories match the canonical base and source references.
- No tiny detail is critical to identity if it disappears at pet size.

## Animation Completeness

- Each row uses the exact expected number of frames.
- The first and last frames can loop without an obvious pop.
- Directional rows read as the intended direction.
- State-specific actions are recognizable at pet size.
- Poses are generated animation variants, not repeated copies of the same source image.

## App Fitness

- First idle frame works as a static reduced-motion pet.
- The `idle` row should be calm and low-distraction; reject it if it reads as waving, walking, running, jumping, talking, working, reviewing, reacting dramatically, changing props, or making large pose/silhouette changes.
- No important detail is too small to read.
- No frame is clipped by the cell.
- Failed/review/waiting states are distinct from ordinary idle.
- Contact sheets must show whole sprite poses inside cells, not cropped tiles from a larger reference image.
- Contact sheets must not be accepted if every used frame is just the reference image with small geometric transforms.
- Used cells must not have white or opaque rectangular backgrounds unless the pet intentionally fills the whole cell and the user accepts that tradeoff.
- The chroma key must be visually absent from the character. If extraction removes character regions, choose a different key and regenerate the affected base/rows.
- Contact sheets must not show edge slivers or partial neighboring sprites inside cells.
- Contact sheets must not show darker/lighter versions of the chroma key as shadows, dust, smears, glows, landing marks, or motion effects. These are background extraction failures and should trigger row repair.
- Reject pixel-art/cartoon drift, reference identity drift, breed/body-type changes, altered markings, changed accessory side, or inconsistent real texture even when geometry QA passes.
- If `qa/review.json` reports edge pixels, sparse frames, size outliers, or slot-extraction fallback, inspect the row visually and repair it when the issue is visible.
- If `qa/review.json` reports chroma-adjacent non-transparent pixels, repair the row unless those pixels are an intentional character color and the selected key was manually accepted.

## Repair Policy

Repair the smallest failing scope first:

1. Single bad frame.
2. One row.
3. Full atlas regeneration only when identity or layout is broadly broken.

The normal production path should queue targeted repair jobs for failing rows. Manual repair should preserve the same run directory and regenerate only the affected row prompt/image unless the base character is wrong.
