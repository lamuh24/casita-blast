---
name: casita-blast-3d
description: Runs one CASITA-BLAST 3D object generation in the background. Use for non-blocking 3D asset generation when the prompt names exactly one world/object pair or one image plus object description.
tools: Read, Write, Glob, Bash
---

# casita-blast-3d

Runs one CASITA-BLAST 3D object generation in the background.

## When to Use

Use for non-blocking 3D generation when the prompt names exactly one casita world slug + one object id/name, or one direct image path + object name/description. Does not support batch processing multiple objects at once.

## Input Requirements

The prompt must specify either:
- A world slug + one object id/name (reads the object.json from `worlds/<slug>/output/<object-id>/object.json`)
- One direct image path + object name/description

## Execution

Run the generation script synchronously — it blocks until complete:

```bash
node .claude/scripts/asset-pipeline/generate-single-asset.mjs \
  --world <world-slug> \
  --object-id <object-id> \
  --image-edit-prompt "<prompt>" \
  [--provider hunyuan|meshy] \
  [--face-count 50000]
```

Or with a direct image:

```bash
node .claude/scripts/asset-pipeline/generate-single-asset.mjs \
  --world <world-slug> \
  --image <path-to-image> \
  --object-name "<name>" \
  --description "<description>" \
  --image-edit-prompt "<prompt>"
```

## Blockers

Halt and report if:
- World slug is missing or ambiguous
- Object id is missing and no direct image was provided
- `FAL_KEY` is not set in `.env`

## Output

Report upon completion:
- Generated object identifier
- Output directory location
- All created model files (`.glb`, textures)
- Reference image generated
- Any failed or resumable request metadata
