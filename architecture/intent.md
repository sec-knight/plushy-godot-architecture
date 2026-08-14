# Intent

## What this project is

A Godot recreation of the existing Plushy browser demo — a small, playful action
game built around a plush mascot — playable on the web from sneaker.games,
including on phones and tablets.

The recreation is deliberately not a literal port. Each behavior of the existing
demo is preserved, improved, or replaced on purpose. The baseline being worked
from is recorded in `CURRENT-DEMO-FEATURE-MAP.md`.

## What the project is for

Two goals run in parallel, and both matter:

1. **The game.** Reproduce the demo's playful plush-toy identity in Godot, then
   improve it through playtesting rather than speculative scope growth.
2. **The loop.** Use this project as the pilot for a complete Grimoire
   development loop — architecture as canon, activity as evidence, deliberate
   promotion between them. A working game with no usable continuity record is a
   partial success at best.

## Identity to protect

- Soft, silly, expressive presentation over realism.
- Immediate arcade-like response over simulation fidelity.
- Cozy beep-boop procedural audio, with small variations so repeated actions
  never sound identical.
- Charm and readability over visual complexity.

## Constraints

- **Web-first.** Godot Web export constraints are an architecture input from the
  start, not a release-time surprise.
- **Touch is first-class.** Phone and tablet play is a primary target because the
  demo is meant to be shared and played with family, not a late compatibility
  pass.
- **Small scope, honestly bounded.** One attack and one dodge before any
  additional abilities; 2–3 differentiated enemies rather than many shallow ones.

## Repositories

| Role | Repository |
| --- | --- |
| Architecture (this repo) | `sec-knight/plushy-godot-architecture` |
| Source | `sec-knight/plushy-godot-source` |
| Archive | `sec-knight/plushy-godot-archive` |

Routing lives in the personal Grimoire's `projects.yaml` under `plushy-godot`.
Exported builds are artifacts and belong in neither architecture nor source.
Screenshots, superseded builds, and raw feedback go to the archive once they stop
earning their place in active architecture.
