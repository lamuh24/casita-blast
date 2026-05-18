---
name: casita-blast-plate
description: Runs one CASITA-BLAST clean plate operation in the background. Removes staged props, construction debris, cars, or other distractions from casita source photography to produce a pristine presentation-ready image.
tools: Read, Write, Glob, Bash
---

# casita-blast-plate

Runs one CASITA-BLAST clean plate / source cleanup operation in the background.

## When to Use

Use for non-blocking removal of confirmed objects or specified content from casita world source images. Required:
- One world slug (mandatory)
- Source image path (optional — defaults to latest in `worlds/<slug>/source/`)
- Removal instructions (optional — defaults to removing anything that isn't the casita structure itself)

If the world slug is missing or the request is ambiguous, halt and flag it.

## Common Casita Plate Requests

- Remove construction debris, scaffolding, temporary fencing
- Remove vehicles, dumpsters, porta-potties
- Remove overgrown weeds or raw dirt — leave graded earth or simple gravel
- Remove staging props not part of the final design
- Neutralize distracting backgrounds (neighboring structures, power lines)

## Execution

Uses `casita-blast-image-edit` internally:

```bash
node .claude/scripts/asset-pipeline/image-edit.mjs \
  --image "worlds/<slug>/source/0-<slug>.<ext>" \
  --prompt "<removal instructions>" \
  --output-dir "worlds/<slug>/source" \
  --provider nano-banana
```

## Output

Report upon completion:
- Input image path
- Output plate image path (new indexed file in source/)
- Edit prompt used
- Request metadata path
