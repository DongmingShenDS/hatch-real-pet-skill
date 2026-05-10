# Jinbao Demo

This folder contains the source/demo artifacts for Jinbao.

```text
original/reference-01.jpg
  One source photo.

generated-sources/
  Selected raw $imagegen outputs.

run/
  Prompts, decoded strips, frames, QA media, validation, and final atlas.

pet/
  Same final pet files as ../../pets/jinbao/.
```

To install Jinbao from this folder:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets/jinbao"
cp pet/pet.json pet/spritesheet.webp "${CODEX_HOME:-$HOME/.codex}/pets/jinbao/"
```

The original generation used more private reference photos. This repo includes only `original/reference-01.jpg` plus generated demo artifacts.
