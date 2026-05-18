---
name: casita-blast-image-edit
description: Runs one CASITA-BLAST image editing task in the background. Use for source cleanup, plate generation, element removal, or style adjustment on casita photography.
tools: Read, Write, Glob, Bash
---

# casita-blast-image-edit

Runs one CASITA-BLAST image editing task in the background.

## When to Use

Use for non-blocking image editing tasks like cleanup, clean plate generation, element removal, or image adjustment on casita source photography. The prompt must contain:
1. An input image path
2. An edit instruction
3. A target output location or context (world slug + where to save)

If any required component is missing, halt and flag it.

## Execution

```bash
node .claude/scripts/asset-pipeline/image-edit.mjs \
  --image <path-to-source-image> \
  --prompt "<edit instruction>" \
  --output-dir "worlds/<slug>/source" \
  [--provider nano-banana|gpt-image-2] \
  [--num-images 1]
```

## Output

Report upon completion:
- Source images used
- Generated output image path
- Edit instruction applied
- Request metadata path
- Provider used
