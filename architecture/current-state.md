# Current State

**As of:** 2026-08-13

## Summary

Design baseline is captured. Implementation has not started. The Godot project
exists only as an empty seed.

## Architecture repository

Grimoire structure is in place: `AGENTS.md`, `FAMILIAR.md`, `architecture/`, and
`activity/`. The demo baseline and Godot recreation targets are recorded in
`CURRENT-DEMO-FEATURE-MAP.md`. No design decisions have been superseded yet, so
there is no `decisions.md`.

## Source repository

`sec-knight/plushy-godot-source` contains a bare Godot project seed —
`project.godot`, `icon.svg`, `icon.svg.import`, LICENSE, README. No scenes, no
scripts, no input-action definitions, no assets from the browser demo.

## Existing browser demo

Still the only playable artifact. It is the behavioral reference for the
recreation. Its source is not part of this project's repositories; the feature
map is the durable record of what it does.

## Milestone 1 — reproduce the recognizable core

Not started. From `CURRENT-DEMO-FEATURE-MAP.md`, all ten criteria remain open:

1. Plushy can move.
2. Plushy can attack and dodge.
3. At least one enemy interacts through damage/defeat rules.
4. Hurt and healing feedback work.
5. Playful procedural SFX identity is present.
6. A simple beep-boop background track is present.
7. Keyboard controls work.
8. Touch controls work on a phone-sized viewport.
9. The project exports successfully to Godot Web.
10. The build can be hosted and launched from sneaker.games.

## Known environment facts

- The SSH remotes recorded in the personal Grimoire's `projects.yaml` are not
  usable from the current Windows machine; HTTPS remotes work. This is a local
  credential gap, not a repository problem.
- The archive repository has not been cloned or inspected.

## Open questions

Carried unchanged from `CURRENT-DEMO-FEATURE-MAP.md` — none are answerable
before there is something playable in Godot. The two most likely to shape early
work are the touch-control scheme and how much procedural audio the Web export
can carry.
