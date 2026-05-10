# Hatch Real Pet

Create a realistic Codex pet from real reference photos. No pixel art.

[中文说明](README.zh-CN.md)

This repo includes:

- `skills/hatch-real-pet/`: the Codex skill.
- `pets/dbao/`: a ready-to-import demo pet.
- `data/dbao/`: DBAO source/demo artifacts for inspection.

Name note: this demo may have appeared as `jinbao` in early internal runs. The public repo uses `DBAO`, the cat's real name, everywhere to avoid confusion. The Codex pet id and folder are lowercase `dbao`; the displayed pet name is `DBAO`.

![DBAO source photo](data/dbao/original/reference-01.jpg)

![DBAO QA contact sheet](data/dbao/run/qa/contact-sheet.png)

## Install The Skill

From this repo root:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/hatch-real-pet "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Restart Codex or start a new session. The skill should appear as `$hatch-real-pet`.

## Create Your Own Pet

Give Codex one or more clear pet photos and ask:

```text
Use $hatch-real-pet to create a realistic Codex pet named Mochi from:
/absolute/path/to/mochi-front.jpg
/absolute/path/to/mochi-side.jpg

Keep the real markings, face, body shape, fur texture, and collar placement.
Package it under my Codex custom pets folder.
```

Output goes here:

```text
${CODEX_HOME:-$HOME/.codex}/pets/<pet-name>/
  pet.json
  spritesheet.webp
```

## Import The DBAO Demo

From this repo root:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets"
cp -R pets/dbao "${CODEX_HOME:-$HOME/.codex}/pets/"
```

Restart Codex or refresh custom pets. DBAO is useful as a quick smoke test because `pets/dbao/` already has the exact two files Codex needs:

```text
pet.json
spritesheet.webp
```

## What The Skill Does

`$hatch-real-pet` uses `$imagegen` for visual generation, then deterministic scripts to package the pet:

1. Generate a realistic base cutout from the reference photo.
2. Generate animation rows for idle, running, jumping, waiting, review, and failure states.
3. Validate the `1536x1872` spritesheet atlas with `192x208` cells.
4. Produce QA images/videos.
5. Write `pet.json` and `spritesheet.webp`.

## Demo Files

- `pets/dbao/`: direct import folder.
- `data/dbao/original/reference-01.jpg`: one source photo.
- `data/dbao/run/qa/contact-sheet.png`: visual QA overview.
- `data/dbao/run/qa/videos/`: animation previews.
- `data/dbao/run/prompts/`: prompts used for the demo.
- `data/dbao/run/final/spritesheet.webp`: final atlas.

The original DBAO generation used additional private references. This repo includes only one source photo plus the generated demo artifacts.
