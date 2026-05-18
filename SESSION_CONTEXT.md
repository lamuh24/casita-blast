# Session Context — NuGen Casitas Real Estate Viewer

## Last Updated
Agent: Claude Code
Date: 2026-05-17

---

## What Was Done This Session

### Walk mode removed
- Stripped `Physics`, `Player`, `MobileControls`, `viewMode` state from `Experience.tsx`
- Viewer is now pure orbit/spin — cleaner, faster, no physics overhead
- `App.tsx` updated accordingly

### Mobile-first SpinViewer built (`src/components/SpinViewer.tsx`)
- Lightweight mobile viewer: no physics, no postprocessing, dpr capped at 1.5, shadows off
- Bottom sheet with 3 tabs: Exterior, Floor, Model
- Collapsed state shows quick swatch row + Quote button
- Exterior color swaps work in real time via `exteriorColor` prop on GLB shell
- AR button: Android fires Scene Viewer intent with existing GLB (live), iOS stub (needs USDZ)
- "Full 3D" chip escalates to Experience.tsx on demand
- `App.tsx` routes mobile → SpinViewer, desktop → Experience

### Real HDRI loaded
- Downloaded `venice_sunset_1k.hdr` from Polyhaven (CC0)
- `app/public/assets/realism/hdr/venice_sunset_1k.hdr` (1.4 MB)
- Both viewers now use `<Environment files="...hdr" />` — real IBL on all surfaces

### 4D time-of-day feature built (key differentiator over Konfigear)
- Replaced day/night toggle with a time-of-day slider (0–24h, default 3pm)
- `sunParams(hour)` computes sun position arc, color, intensity, hemisphere, fog, sky as continuous functions
- `SceneLighting` component renders Sky + HDRI + all lights from time value
- Shadow-casting directional light tracks the sun — shadows move in real time
- Interior point light gets brighter as it gets dark outside
- Slider UI in bottom toolbar with dawn/sunset icons + live clock readout

### `RealismHomeShell` now accepts `exteriorColor` prop
- Traverses GLB, sets `material.color` on non-roof meshes at runtime
- Powers the material color swap in SpinViewer

---

## THE CORE PROBLEM (not yet fixed)

The 3D home models are **procedurally generated boxes** from `app/scripts/build-realism-assets.mjs`.
No amount of lighting will fix box geometry. The GLBs need to be replaced with real models.

**Files to replace:**
- `app/public/assets/realism/models/home-shell-20ft.glb`
- `app/public/assets/realism/models/home-shell-30ft.glb`
- `app/public/assets/realism/models/home-shell-40ft.glb`

---

## NUGEN CASITAS ULTIMAT — CASITA-BLAST Skill (INSTALLED 2026-05-17)

The image-blaster skill has been cloned and fully adapted as **CASITA-BLAST** for this project.

### What was installed
- `.claude/rules/project.md` — CASITA-BLAST brain (customized for casita real estate)
- `.claude/agents/casita-blast-{3d,world,sfx,image-edit,plate,wildcard}.md` — 6 agents
- `.claude/hooks/{input-check,setup-check}.sh` — session hooks
- `.claude/scripts/` — full asset pipeline (19 scripts: fal-queue, world gen, SFX, 3D, image edit, project utils)
- `.env.example` — copy to `.env` and fill in `WORLD_LABS_API_KEY` and `FAL_KEY`
- `input/` — staging directory for source photos

### How to use CASITA-BLAST
1. Drop a casita photo into `input/`
2. Say "casita-blast it" — I'll generate a 3D environment, SFX, and 3D assets
3. Results load in the viewer at `localhost:5173/<slug>`

### API Keys needed
- `WORLD_LABS_API_KEY` from https://platform.worldlabs.ai/
- `FAL_KEY` from https://fal.ai/

### FIRST CASITA-BLAST RUN — nugen-20ft (COMPLETE 2026-05-17)

Source image: `worlds/nugen-20ft/source/0-nugen-20ft.png`
All assets generated and on disk:

| Asset | Path | Notes |
|---|---|---|
| World GLB | `worlds/nugen-20ft/output/world/0-world.glb` | World Labs Marble 1.1 |
| Gaussian splats | `worlds/nugen-20ft/output/world/0-world-*.spz` | 4 LODs |
| World panorama | `worlds/nugen-20ft/output/world/0-world-pano.png` | |
| World thumbnail | `worlds/nugen-20ft/output/world/0-world-thumbnail.webp` | Casita + palms + lawn |
| Palm tree GLB | `worlds/nugen-20ft/output/palm-tree/0-palm-tree.glb` | 28MB, Hunyuan 3D v3 |
| Entry planter GLB | `worlds/nugen-20ft/output/entry-planter/0-entry-planter.glb` | Hunyuan 3D v3 |
| Ambient SFX | `worlds/nugen-20ft/output/sfx/0-ambience.mp3` | 7s loop, tropical breeze |

**Windows gotcha:** `node script.mjs` entry-point guard fails on Windows — `import.meta.url` uses forward slashes, `process.argv[1]` uses backslashes. Fix: call module exports directly via `node -e "import('./script.mjs').then(async m => { await m.exportedFn({...}); })"` instead of running scripts as CLI entry points.

---

## NEXT SESSION — Build Real Models in Blender via MCP (NEW PC)

This is the priority task for the next session on the more powerful PC.

### Blender MCP Setup (do this first on new PC)
1. Install Blender 4.x — https://www.blender.org/download/
2. Install Blender MCP addon:
   - Download from https://github.com/ahujasid/blender-mcp
   - Blender: Edit → Preferences → Add-ons → Install → select `addon.py` → enable
   - Press N in viewport → MCP tab → Start MCP Server
3. Add to Claude Code MCP config (`~/.claude/settings.json`):
   ```json
   "blender": {
     "command": "uvx",
     "args": ["blender-mcp"]
   }
   ```
   Install uvx if needed: `pip install uv`
4. Restart Claude Code → tell Claude "Blender MCP is connected, let's build the NuGen Casita models"

### What Claude will build in Blender
Three GLB files — 20ft, 30ft, 40ft — each with these **named mesh groups** (critical for material swap):
- `frame` — black powder-coated steel I-beam structure at corners and edges
- `cladding` — flat wall panels with visible seam lines
- `roof` — slightly pitched, matte black
- `windows` — aluminum frames + glass panes
- `door` — front entry with frame
- `foundation` — base sill/rail

### Reference to have ready
Screenshot 3-4 frames from `WhatsApp Video 2026-05-13 at 4.47.03 PM.mp4`:
- Front elevation (straight on)
- Corner angle (shows frame detail)
- Roof line
- Side elevation

---

## After Models Are Done

1. **iOS AR** — run Apple Reality Converter on the GLBs → `.usdz` files → AR button fully live on iOS
2. **360 spin renders** — render 36 angles per variant in Blender Cycles for marketing hero images
3. **Camera fly-in animation** — dramatic load animation when viewer opens (code only, no model needed)

---

## Key Architecture

| Concern | File |
|---------|------|
| Routing / mobile detection | `src/App.tsx` |
| Desktop viewer (orbit, 4D sun, post-processing) | `src/components/Experience.tsx` |
| Mobile viewer (lightweight, AR, bottom sheet) | `src/components/SpinViewer.tsx` |
| 3D scene assembly + materials + add-ons | `src/components/House.tsx` |
| GLB loading + exteriorColor prop | `src/components/RealismAssets.tsx` |
| Plan dimensions (20/30/40ft walls) | `src/data/floorplans.ts` |
| Procedural texture generators | `src/utils/textures.ts` |

**Material swap entry point:** `RealismHomeShell` in `RealismAssets.tsx` — accepts `exteriorColor?: string`

**4D sun function:** `sunParams(hour: number)` in `Experience.tsx`

**Mobile detection:** `isTouchDevice()` from `src/store.ts`

**HDRI path:** `app/public/assets/realism/hdr/venice_sunset_1k.hdr`

**Deploy:** Netlify → `tiny-home-viewer-demo.netlify.app`

---

## Gotchas

- **Build from** `C:\real estate viewer\app` — NOT the `C:\NuGen Casitas` junction (Vite bug)
- **Lint first:** `npx tsc --noEmit` before every commit
- **Mesh naming is critical** — `frame`/`cladding`/`roof`/`windows`/`door` — if names are wrong, color swap targets wrong surfaces
- **GitHub remote** is HTTPS now: `https://github.com/lamuh24/NuGen-Casitas-.git` (was SSH, changed because SSH keys not configured in dev environment)
- **`app/dist` deleted** during cleanup — run `npm run build` before Netlify deploy
- **HDR file** is 1.4MB, already committed, don't re-download
- The `C:\NuGen Casitas` path is a Windows directory junction pointing to `C:\real estate viewer` — both work for browsing but only the real path works for Vite builds
