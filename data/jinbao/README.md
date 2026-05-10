# Jinbao Demo Data

This folder is a complete demo artifact set for a realistic Codex pet named Jinbao.

```text
original/reference-01.jpg
  One source photo included for demonstration.

generated-sources/
  Selected raw $imagegen outputs for the base and row strips.

run/
  The working run directory: prompts, decoded strips, extracted frames,
  QA media, validation output, and final spritesheet.

pet/
  Ready-to-install Codex custom pet files.
```

The original generation used more than one private reference photo. This repository includes only `original/reference-01.jpg` and the generated/processed artifacts needed to demonstrate the workflow.

To install the demo pet:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets"
cp -R pets/jinbao "${CODEX_HOME:-$HOME/.codex}/pets/"
```

From inside this folder, the equivalent command is:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/pets/jinbao"
cp pet/pet.json pet/spritesheet.webp "${CODEX_HOME:-$HOME/.codex}/pets/jinbao/"
```
