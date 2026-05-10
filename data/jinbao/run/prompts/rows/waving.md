Create a single horizontal realistic cutout sprite strip for the Codex app real pet `jinbao` in the state `waving`.

Use the attached reference image(s) for real pet identity and the attached base pet image as the canonical design. Use the attached layout guide image only for frame count, slot spacing, centering, and safe padding. Preserve realistic reference details enough for the pet to remain the same individual at 192x208. Do not simply copy the still reference pose. Generate distinct natural animation poses that create a readable cycle.

Identity lock:
- Do not redesign the pet. Only change pose/action for the `waving` animation.
- Preserve the exact head shape, ear/horn/limb shape, face, markings, realistic colors, fur/skin/feather/material texture, body proportions, accessory placement, and overall silhouette from the canonical base pet.
- Keep every frame recognizably the same individual pet, not a related variant.
- If the pet has a prop or accessory, preserve its size, side, realistic colors, and attachment style unless the row action requires a small pose-only adjustment.
- Prefer a subtler animation over any change that mutates the pet identity.

Output exactly 4 separate animation frames arranged left-to-right in one single row. Each frame must show the same pet: Reference identity: golden shaded short-haired cat, likely British Shorthair type; very round face and cheeks; large green eyes; small pink nose; cream white muzzle and chin; warm golden head and chest; darker brown shaded back and tail with subtle tabby stripes; compact plush body; short upright ears; long white whiskers; soft realistic fur texture. Preserve the same cat, not a cartoon or pixel mascot..

Style contract: Real Codex pet cutout style: realistic reference-faithful full-body pet, natural proportions, real fur/skin/feather/material texture, preserved markings, preserved face and expression language, preserved accessory placement, and crisp clean cutout edges suitable for chroma-key extraction. Keep the pet compact and readable inside a 192x208 cell while retaining realistic color and texture. Avoid pixel art, chibi proportions, cartoon mascot redesign, anime style, toy-like 3D rendering, vector logo treatment, sticker treatment, dark outline overlays, stepped pixel edges, flat cel shading, generic stock-animal drift, scenery, floor contact, cast shadows, soft shadows, glow, blur, and loose flyaway edge wisps. Additional user style notes: Realistic clean cutout for a small Codex pet, full body compact in each cell, flat chroma-key background, no shadows, no props unless explicitly in the pose..

Use this prompt as an authoritative realistic pet animation spec. Do not convert the pet into pixel art, a cartoon mascot, chibi, anime character, toy-like 3D render, vector logo, sticker, generic stock animal, glossy app icon, or standalone animal portrait.

Animation action: greeting gesture with raised wave and return.


State-specific requirements:
- Show the greeting through paw pose only: paw down, paw raised, paw tilted, paw returning.
- Do not draw wave marks, motion arcs, lines, sparkles, symbols, or floating effects around the paw.

Transparency and artifact rules:
- Prefer pose, expression, and silhouette changes over decorative effects.
- Effects are allowed only when they are state-relevant, opaque, clean-edged, fully inside the same frame slot, and physically touching or overlapping the pet silhouette.
- Allowed attached effects can include a tear touching the face, a small smoke puff touching the pet or prop, or tiny stars overlapping the pet during a failed/dizzy reaction, but only when they do not make the pet look cartoonish.
- Do not draw detached effects: floating stars, loose sparkles, floating punctuation, floating icons, falling tear drops, separated smoke clouds, loose dust, disconnected outline bits, or stray artifacts.
- Do not draw wave marks, motion arcs, speed lines, action streaks, afterimages, blur, smears, halos, glows, auras, floor patches, cast shadows, contact shadows, drop shadows, oval floor shadows, landing marks, or impact bursts.
- Do not include text, labels, frame numbers, visible grids, guide marks, speech bubbles, thought bubbles, UI panels, code snippets, scenery, checkerboard transparency, white backgrounds, or black backgrounds.
- Do not use the chroma-key color or chroma-key-adjacent colors in the pet, prop, effects, highlights, shadows, or edges.
- Reject any pose that is cropped, overlaps another pose, crosses into a neighboring frame slot, or creates a separate disconnected component that is not attached to the pet.

Layout requirements:
- Exactly 4 full-body frames, left to right, in one horizontal row.
- The attached layout guide shows the 4 frame boxes and inner safe area for this row. Follow its slot count, spacing, centering, and padding.
- Do not reproduce the layout guide itself: no visible boxes, guide lines, center marks, labels, guide colors, or guide background may appear in the output.
- Treat the image as 4 equal-width invisible frame slots. Fill every slot: each requested slot must contain exactly one complete full-body pose.
- Spread the 4 poses evenly across the whole image width. Do not leave any requested slot blank or create large empty gaps between poses.
- Center one complete pose in each slot. No pose may cross into the neighboring slot.
- Use a perfectly flat pure blue #0000FF chroma-key background across the whole image.
- Do not draw visible grid lines, borders, labels, numbers, text, watermarks, or checkerboard transparency.
- Do not include scenery or a background environment.
- Keep the rendering as a realistic clean cutout: natural texture, reference-faithful markings, crisp subject edges, compact full-body silhouette, and no artificial dark outline overlay.
- Do not use #0000FF, pure blue, or colors close to that chroma key in the pet, props, highlights, shadows, motion marks, dust, landing marks, edges, or effects.
- Do not draw shadows, glows, smears, dust, or landing marks using darker/lighter versions of the chroma-key color.
- Keep every frame self-contained with safe padding. No pet body part should be clipped by the frame slot.
- Avoid motion blur and loose flyaway edges. Use clear natural pose changes readable at 192x208.
- Preserve the same silhouette, face, proportions, realistic colors, material texture, and props across every frame.
