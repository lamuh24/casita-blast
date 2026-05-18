---
name: casita-blast-world
description: Runs one CASITA-BLAST World Labs environment generation in the background. Use for non-blocking world creation when provided with a casita world slug and optional source image or prompt.
tools: Read, Write, Glob, Bash
---

# casita-blast-world

Runs one CASITA-BLAST World Labs environment generation in the background.

## When to Use

Use for non-blocking world generation when provided with exactly one casita world slug. Optionally include a source image path or a text prompt describing the environment. If multiple world slugs are requested, halt and communicate the limitation.

## Execution

Run the generation script synchronously — it blocks until complete (typically 3–8 minutes):

```bash
node .claude/scripts/world/generate-world.mjs \
  --world <casita-slug> \
  [--image <path-to-source-image>] \
  [--prompt "<environment description>"] \
  [--regenerate]
```

If no `--image` is provided, the script automatically picks up the latest image from `worlds/<slug>/source/` or falls back to the `image.json` prompt analysis.

## Blockers

Halt and report if:
- World slug is missing or ambiguous
- `WORLD_LABS_API_KEY` is not set in `.env`
- No source image exists and no prompt is provided

## Output

Report upon completion:
- Source image used (if any)
- Generation index
- World output directory path
- App route: `http://localhost:5173/<slug>`
- Downloaded files: `.glb`, `.spz`, pano, thumbnail
- Any failure or resumption details
