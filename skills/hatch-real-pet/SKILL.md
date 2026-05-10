---
name: hatch-real-pet
description: Create, repair, validate, preview, and package Codex-compatible animated realistic pets and real-pet spritesheets from reference photos or realistic concept images. Use when a user wants to hatch a real pet, create a non-pixel Codex pet, preserve an animal from supplied images, generate realistic cutout animation rows, build an 8x9 Codex pet atlas with transparent unused cells, produce QA contact sheets and preview videos, and save pet.json plus spritesheet.webp under the Codex custom pets folder. This skill composes the installed $imagegen system skill for visual generation and uses bundled deterministic scripts for manifest, frame extraction, validation, atlas assembly, previews, and packaging.
---

# Hatch Real Pet

## Overview

Create a Codex-compatible animated pet that keeps a realistic, reference-faithful look instead of the default pixel-art house style. The final output still follows the Codex custom pet contract: a `1536x1872` atlas, `192x208` cells, transparent unused cells, `pet.json`, and `spritesheet.webp`.

This skill is reference-first. If the user asks for a real pet but provides no reference image, ask for one unless they explicitly authorize a concept-only realistic pet. When references exist, treat them as identity ground truth: species, breed, body shape, face, markings, fur/skin/feather texture, colors, collar/accessory placement, and distinctive asymmetries must be preserved.

If the user omits a pet name, infer one from the concept or reference filenames; if that is not possible, choose a short friendly name. If the user omits a description, infer one from the concept or references.

## Generation Delegation

Use `$imagegen` for all normal visual generation.

Before generating base art, row strips, or repair rows, load and follow:

```text
${CODEX_HOME:-$HOME/.codex}/skills/.system/imagegen/SKILL.md
```

Do not call the Image API directly for the normal path. Let `$imagegen` choose its built-in-first path and CLI fallback policy. If `$imagegen` says a fallback requires confirmation, ask the user before continuing.

Use this skill's scripts only for deterministic work: prompt/manifest preparation, ingesting selected `$imagegen` outputs, extracting frames, validation, atlas assembly, QA media, and packaging.

Hard boundary: do not draw, tile, warp, repaint, mirror, or synthesize pet visuals with local Python/Pillow scripts, SVG, canvas, HTML/CSS, or other code-native art as a substitute for `$imagegen`. The only visual derivation exception is `running-left`, which may be created by mirroring `running-right` only after `running-right` is generated, visually inspected, and explicitly judged safe to mirror.

For the built-in path, record selected source images from `$CODEX_HOME/generated_images/.../ig_*.png`. Do not record files from the run directory, `tmp/`, hand-made fixtures, deterministic row folders, or post-processed copies as visual job sources.

## Real Pet Style

The target look is a realistic transparent cutout pet suitable for small app animation:

- Preserve the referenced individual animal rather than converting it into a mascot, chibi, cartoon, anime, toy, 3D render, sticker, logo, or pixel-art sprite.
- Use natural proportions, realistic fur/skin/feather/material texture, real markings, real eye/nose/mouth details, and reference-faithful colors.
- Keep the full body visible and compact enough to read inside a `192x208` cell.
- Prefer crisp realistic cutout edges with a clean silhouette. Avoid flyaway fur, translucent wisps, soft glows, soft shadows, floor contact, scenery, or anything that makes chroma-key removal unreliable.
- Keep realism, but simplify only enough for readability at pet size. Do not add dark pixel outlines, stepped edges, flat cel shading, limited palettes, or fake sprite-style effects.

## Transparency And Effects

Pet rows are processed into transparent cells, so every generated pixel must either belong to the pet or be cleanly removable chroma-key background.

Avoid decorative effects by default. If an effect is necessary for a state, it must be opaque, realistic or cleanly graphic, physically touching or overlapping the pet silhouette, inside the same frame slot, and small enough to remain readable at `192x208`.

Do not generate detached stars, sparkles, punctuation, icons, smoke clouds, dust, wave marks, speed lines, action streaks, afterimages, blur, smears, halos, glows, auras, cast shadows, contact shadows, drop shadows, oval floor shadows, floor patches, landing marks, impact bursts, labels, frame numbers, grid lines, UI, code, scenery, checkerboard transparency, white backgrounds, or black backgrounds.

State-specific guidance:

- `idle`: subtle breathing, blink, small head/body bob, tail/ear sway, or calm realistic motion only.
- `waving`: show the gesture through a paw/limb pose only; no wave marks or symbols.
- `jumping`: show vertical motion through body position only; no floor shadows, dust, or landing marks.
- `failed`: use slumped pose, drooping ears/limbs, sad eyes, or lowered posture. Attached tears or tiny attached smoke/stars are allowed only when clean and not detached.
- `review`: show focus through lean, blink, narrowed eyes, head tilt, or paw position. Do not add papers, code, UI, magnifiers, or symbols unless that prop is already part of the reference identity.
- `running-right` and `running-left`: show directional locomotion through natural body and limb motion only; no speed lines or dust.
- `running`: show busy/in-progress behavior without directional travel. Do not show literal jogging or sprinting.

## Visible Progress Plan

For every real pet run, keep a visible checklist and update it as each step finishes:

1. Getting `<Pet>` ready.
2. Capturing `<Pet>`'s real look.
3. Picturing `<Pet>`'s poses.
4. Hatching `<Pet>`.

Use the pet's name when available; otherwise use `your pet`.

## Default Workflow

Prepare a run folder and imagegen job manifest:

```bash
SKILL_DIR="${CODEX_HOME:-$HOME/.codex}/skills/hatch-real-pet"
python "$SKILL_DIR/scripts/prepare_pet_run.py" \
  --pet-name "<Name>" \
  --description "<one sentence>" \
  --reference /absolute/path/to/reference.png \
  --output-dir /absolute/path/to/run \
  --pet-notes "<stable real-pet identity notes>" \
  --style-notes "<optional realistic style notes>" \
  --force
```

For reference-based pets, pass every useful source image with `--reference`. For concept-only realistic pets, pass the concept through `--pet-notes`, omit `--reference`, and add `--allow-concept-only` only if the user explicitly allowed that.

Inspect ready jobs:

```bash
python "$SKILL_DIR/scripts/pet_job_status.py" --run-dir /absolute/path/to/run
```

For each ready job, invoke `$imagegen` with the prompt file listed in `imagegen-jobs.json` and every input image listed for the job, with role labels. The base job completes first. After recording the base, row jobs must include the original references, `references/canonical-base.png`, `decoded/base.png`, and the row's `references/layout-guides/<state>.png`.

Record the selected `$imagegen` output:

```bash
python "$SKILL_DIR/scripts/record_imagegen_result.py" \
  --run-dir /absolute/path/to/run \
  --job-id <job-id> \
  --source /absolute/path/to/generated-output.png
```

Generate and record `running-right` before deciding how to complete `running-left`. Mirror `running-left` only when a horizontal flip preserves identity, markings, lighting cues, accessory side, handed props, and direction semantics:

```bash
python "$SKILL_DIR/scripts/derive_running_left_from_running_right.py" \
  --run-dir /absolute/path/to/run \
  --confirm-appropriate-mirror \
  --decision-note "<why mirroring preserves this pet's real identity>"
```

If there is any side-specific marking, readable text, non-mirrored logo, handed prop, one-sided accessory, or directional visual cue that would become wrong when flipped, generate `running-left` as a normal grounded `$imagegen` row.

When all jobs are complete, finalize:

```bash
python "$SKILL_DIR/scripts/finalize_pet_run.py" \
  --run-dir /absolute/path/to/run
```

Review `qa/contact-sheet.png`, `qa/review.json`, `final/validation.json`, and `qa/videos/` before accepting the pet. Deterministic validation is necessary but not sufficient: visually reject any row that changes the animal's identity, breed/body type, face, markings, fur texture, accessory placement, scale, or overall silhouette.

Package output is written to:

```text
${CODEX_HOME:-$HOME/.codex}/pets/<pet-name>/
  pet.json
  spritesheet.webp
```

## Subagent Row Generation

After the base job has been recorded and `references/canonical-base.png` exists, row-strip visual generation must use subagents unless the user explicitly says not to use subagents for this session. The parent agent owns manifest writes, result recording, mirroring decisions, repairs, finalization, and packaging.

Default order:

1. Parent runs `prepare_pet_run.py`.
2. Parent generates and records `base`.
3. Parent delegates `idle` and `running-right` first as identity and gait checks.
4. Parent records selected row outputs returned by subagents.
5. Parent decides whether `running-left` can be mirrored; otherwise delegates it.
6. Parent delegates remaining row jobs.
7. Parent finalizes, reviews, repairs if needed, and packages.

Subagents must not edit `imagegen-jobs.json`, copy into `decoded/`, run `record_imagegen_result.py`, run `derive_running_left_from_running_right.py`, run `finalize_pet_run.py`, or package the pet.

Use this handoff template:

```text
Generate the `<row-id>` row for this hatch-real-pet run.

Run dir: <absolute run dir>
Prompt file: <absolute prompt file>
Input images:
- <absolute path> — <role>
- <absolute path> — <role>

Read and follow the row prompt exactly. Use `$imagegen` only; do not use local scripts to draw, tile, edit, or synthesize sprites.

Before returning, visually check:
- exact requested frame count
- same real pet identity as the canonical base and source references
- clean flat chroma-key background
- complete, separated, unclipped poses
- no pixel-art/cartoon drift, forbidden detached effects, shadows, or slot-crossing artifacts

Do not edit manifests, copy into decoded, record results, mirror rows, finalize, repair, or package. Return only:
selected_source=/absolute/path/to/$CODEX_HOME/generated_images/.../ig_*.png
qa_note=<one sentence>
```

If subagents cannot be used, stop before row-strip generation and ask for explicit user direction before continuing sequentially.

## Repair Workflow

If finalization stops because row QA failed, queue targeted repair jobs:

```bash
python "$SKILL_DIR/scripts/queue_pet_repairs.py" \
  --run-dir /absolute/path/to/run
```

Repeat `$imagegen` generation and `record_imagegen_result.py` for each reopened row. Regenerate the smallest failing scope: the failed row, not the whole sheet. For identity repairs, use the canonical base image, original references, contact sheet, and exact row failure note as grounding context.

## Rules

- Keep `$imagegen` as the primary generation layer.
- Keep reference images attached/visible for `$imagegen` whenever the chosen path supports references.
- Use the row's layout guide as spacing input only; reject outputs that copy guide pixels.
- Generate every normal visual job with `$imagegen`: base plus all row strips that are not approved `running-left` mirrors.
- Treat only the base job as eligible for prompt-only generation, and only for explicitly approved concept-only runs.
- Do not substitute locally drawn, tiled, transformed, or code-generated row strips for missing `$imagegen` outputs.
- Never manually mutate `imagegen-jobs.json` to claim a visual job completed.
- Use the chroma key stored in `pet_request.json`; do not force a fixed green screen.
- Preserve the real pet's species, breed/body shape, face, markings, texture, colors, accessories, and asymmetries across all rows.
- Treat visual identity drift as a blocker even when `qa/review.json` and `final/validation.json` have no errors.
- Treat a contact sheet that shows cropped references, repeated tiles, opaque cell backgrounds, non-pet fragments, pixel-art/cartoon drift, or mismatched animal identity as failed.
- Treat `qa/review.json` errors as blockers. Warnings require visual review.

## Acceptance Criteria

- Final atlas is PNG or WebP, `1536x1872`, transparent-capable, and based on `192x208` cells.
- Used cells are non-empty and unused cells are fully transparent.
- Atlas follows `references/animation-rows.md`.
- Contact sheet and preview videos have been produced unless explicitly skipped.
- `qa/review.json` has no errors.
- Row-by-row visual review confirms the pet remains the same realistic individual and the animation cycles are complete enough for the Codex app.
- `${CODEX_HOME:-$HOME/.codex}/pets/<pet-name>/pet.json` and `${CODEX_HOME:-$HOME/.codex}/pets/<pet-name>/spritesheet.webp` are created together.
