# DBAO Demo

This folder contains the source/demo artifacts for DBAO.

Name note: early internal run files may have used `jinbao`; this public demo is named `DBAO` consistently because that is the cat's real name. The Codex pet id and folder are lowercase `dbao`; the displayed pet name is `DBAO`.

```text
original/reference-01.jpg
  One source photo.

generated-sources/
  Selected raw $imagegen outputs.

run/
  Prompts, decoded strips, frames, QA media, validation, and final atlas.

pet/
  Same final pet files as ../../pets/dbao/.
```

To install DBAO from this folder:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets/dbao"
cp pet/pet.json pet/spritesheet.webp "${CODEX_HOME:-$HOME/.codex}/pets/dbao/"
```

The original generation used more private reference photos. This repo includes only `original/reference-01.jpg` plus generated demo artifacts.
