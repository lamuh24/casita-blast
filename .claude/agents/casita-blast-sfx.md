---
name: casita-blast-sfx
description: Runs one CASITA-BLAST SFX generation in the background. Use for ambient soundscapes, object sounds, or custom audio targeting a casita world.
tools: Read, Write, Glob, Bash
---

# casita-blast-sfx

Runs one CASITA-BLAST sound effect generation in the background.

## When to Use

Use for non-blocking SFX generation targeting one casita world. Each prompt must specify:
- One world slug
- One SFX target: world ambience, object-specific sound, or a custom SFX prompt

If the world designation or SFX target is unclear, halt and flag it.

## Casita SFX Guidance

For world ambience, lean into the casita lifestyle experience:
- Desert setting: light wind, distant ravens, dry gravel crunch
- Suburban/infill: birdsong, occasional car pass, lawn ambience
- Mountain/hillside: pine breeze, stream, hawk cry
- General: soft breeze, morning birds, quiet neighborhood

Keep ambience loops short (4–8 seconds), unobtrusive, and loopable.

## Execution

World ambience:
```bash
node .claude/scripts/sfx/fal-elevenlabs-sfx.mjs \
  --prompt "<ambient description>" \
  --output-dir "worlds/<slug>/output/sfx" \
  --prefix "ambience" \
  --loop \
  --duration-seconds 6
```

Object sound:
```bash
node .claude/scripts/sfx/fal-elevenlabs-sfx.mjs \
  --prompt "<object sound description>" \
  --output-dir "worlds/<slug>/output/<object-id>/sfx" \
  --prefix "<object-id>" \
  --count 2
```

## Blockers

Halt and report if:
- World slug is missing
- SFX target is unclear
- `FAL_KEY` is not set in `.env`
- `ffmpeg` is not available (required for post-processing)

## Output

Report upon completion:
- Generated audio file paths
- Loop status
- Duration after trimming
- Quality score from audio analysis
- Request metadata path
