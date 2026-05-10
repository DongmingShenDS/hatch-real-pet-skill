# Hatch Real Pet Skill

Create Codex-compatible animated pets from real reference photos.

This repository contains a Codex skill, `hatch-real-pet`, plus a complete Jinbao demo run. The skill is a realistic, reference-first variant of the Codex pet hatching workflow: it avoids pixel art and tries to preserve the actual animal's body shape, face, markings, colors, texture, and accessories across every animation row.

![Jinbao source photo](data/jinbao/original/reference-01.jpg)

![Jinbao QA contact sheet](data/jinbao/run/qa/contact-sheet.png)

## Repository Layout

```text
skills/hatch-real-pet/
  SKILL.md
  agents/openai.yaml
  references/
  scripts/

data/jinbao/
  original/reference-01.jpg          # one source photo
  generated-sources/                 # selected raw $imagegen outputs
  run/                               # decoded strips, frames, prompts, QA, final atlas
  pet/                               # ready-to-install pet.json + spritesheet.webp

pets/jinbao/
  pet.json
  spritesheet.webp                   # direct Codex custom-pet import folder
```

The skill itself lives in `skills/hatch-real-pet/`. The demo data is not required to use the skill; it is included so people can inspect what a finished real-pet run looks like.

## Install Into Codex

Clone this repository, then copy the skill folder into your Codex skills collection:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/hatch-real-pet "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Restart Codex or start a new Codex session so the skill list refreshes. You should then be able to invoke it as `$hatch-real-pet`.

The skill composes the built-in `$imagegen` skill, so image generation must be available in your Codex environment.

## Hatch Your Own Pet

Give Codex at least one clear reference image. Multiple references usually improve identity consistency, but one good full-body or near-full-body image is enough to start.

Example prompt:

```text
Use $hatch-real-pet to create a realistic Codex pet named Mochi from these reference images:
/absolute/path/to/mochi-front.jpg
/absolute/path/to/mochi-side.jpg

Keep the real markings, face, body shape, fur texture, and collar placement.
Package it under my Codex custom pets folder.
```

The skill will:

1. Prepare a run folder and grounded `$imagegen` prompts.
2. Generate a realistic base cutout from your reference image.
3. Generate each animation row as a clean chroma-key strip.
4. Generate `running-right` before deciding whether `running-left` can be mirrored safely.
5. Extract frames, validate the atlas, render QA media, and package the final pet.

The finished pet is written to:

```text
${CODEX_HOME:-$HOME/.codex}/pets/<pet-name>/
  pet.json
  spritesheet.webp
```

## Try The Jinbao Demo Pet

To install the included Jinbao demo pet locally, copy the ready-made Codex pet folder:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets"
cp -R pets/jinbao "${CODEX_HOME:-$HOME/.codex}/pets/"
```

Then restart Codex or refresh custom pets in your Codex app.

## Demo Files

`data/jinbao/run/final/spritesheet.webp` is the final `1536x1872` atlas. Each cell is `192x208`, with 8 columns and 9 animation rows.

`data/jinbao/run/qa/contact-sheet.png` is the fastest visual QA artifact. It shows every extracted frame row-by-row.

`data/jinbao/run/qa/videos/` contains short preview loops for each state.

`data/jinbao/run/prompts/` contains the exact base and row prompts used for the demo.

`data/jinbao/generated-sources/` contains selected raw `$imagegen` outputs before deterministic extraction and assembly.

Only one Jinbao source photo is included under `data/jinbao/original/`. The original run used additional private references; those are intentionally not included here.

## Notes

- This skill is for realistic pets, not pixel sprites.
- Reference identity is more important than generic cuteness.
- The deterministic scripts do not draw or repaint the pet. They prepare prompts, ingest generated outputs, extract frames, validate the atlas, make QA media, and package the result.
- The `running-left` row may be derived by mirroring `running-right` only after visual inspection confirms that mirroring does not break side-specific markings, accessories, or direction semantics.
