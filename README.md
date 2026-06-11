# Casita Blast — Interactive 3D Home Viewer

A browser-based 3D viewer built for [NuGen Casitas](https://nugencasitas.com), a Tampa prefab modular home company. Lets buyers walk around a casita model, restyle it, and see it in their own yard via AR — before anyone pours a foundation.

## Features

- **Real-time exterior color swapping** — materials update live on the GLB shell
- **4D time-of-day lighting** — a 0–24h sun slider drives sun position, color, intensity, fog, and sky as continuous functions; shadows track the sun in real time, interior lights brighten as it gets dark
- **Real HDRI environment** — image-based lighting on all surfaces (Polyhaven, CC0)
- **Mobile-first SpinViewer** — lightweight orbit viewer with no physics or postprocessing, DPR capped, bottom-sheet UI with Exterior / Floor / Model tabs
- **AR on Android** — fires a Scene Viewer intent with the live GLB (iOS USDZ support planned)
- **Adaptive routing** — mobile devices get the SpinViewer, desktop gets the full experience, with an on-demand "Full 3D" escalation

## Stack

React Three Fiber (Three.js) · TypeScript · GLB/GLTF assets · HDRI image-based lighting

## Development workflow

Built with a multi-agent AI workflow (Claude Code + Codex) coordinated through a shared `SESSION_CONTEXT.md` so each agent picks up exactly where the last one left off. Performance-sensitive decisions (physics removal, DPR caps, shadow budgets) were made by hand and verified per device class.

---

Part of [LAMUH Development Services](https://github.com/lamuh24) client work.
