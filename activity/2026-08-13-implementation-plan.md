# Session: Read the shipped demo, propose an implementation plan

**Date:** 2026-08-13
**Environment:** Claude Code

## Goal

Apply the decided fix to the SSH routing defect, then produce a game plan for
implementing the Godot recreation.

## Context Used

`GRIMOIRE.md`, `black-clover/projects.yaml`, this repo's `AGENTS.md`,
`architecture/intent.md`, `architecture/CURRENT-DEMO-FEATURE-MAP.md`,
`architecture/current-state.md`, `activity/CURRENT.md`, and — for the first time
— the actual demo source in `sneaker.games/plushy/`.

## Work

Routing fix applied (finding
`black-clover/findings/2026-08-13-ssh-remotes-unusable.md` now closed):
`projects.yaml` and the Grimoire template record HTTPS only; the workaround notes
here and in `architecture/current-state.md` were cleared.

Read the shipped demo end to end: `plushy/index.html` (~19.5 KB, single-file
canvas game), `plushy/music.js` (WebAudio scheduler + SFX), `plushy/art/`
(RLE-encoded sprite blobs plus runtime PNG/SVG), `plushy/tests/`, and the
`games/plushy-guardian/` launch wrapper.

## Findings

**The demo is substantially further along than the architecture describes.**
`CURRENT-DEMO-FEATURE-MAP.md` was written from memory of the demo, not from its
source, and it undersells it. The shipped game has three levels with distinct
palettes, enemy weights and boss trees; three enemy kinds with per-level size and
behavior variants (`skitter`, `bruiser`); three waves per level; a boss phase
against a tree with its own health bar; seed and stuffing pickups; and a full
welcome → level-intro → end-panel flow. The "initial definition of success" in
`current-state.md` is therefore a deliberate *subset* of what already exists, not
a step forward from it. That is still the right first milestone, but it should be
labelled as a subset so nobody reads a completed milestone 1 as parity.

**The attack is ranged, and aim is implicit.** Architecture says "one primary
attack" without saying what it is. In the demo, `fire()` launches a vine
projectile (465 px/s, r11, 0.9 s life, 20 damage) along `lastAimDirection` —
which is simply the last non-zero movement direction. There is no separate aim
input on either keyboard or touch. This is load-bearing: it is why a wisp sprite
orbits the player at 40 px, and it is the whole reason the game is playable
one-thumbed on a phone. It belongs in architecture.

**The dodge is a burst, not a roll.** `dodge()` sets a 0.7 s cooldown and 0.3 s
of invulnerability, and movement is multiplied by 2.7 only while
`inv > 0 && dodge > 0.39` — roughly the first 0.3 s. The feature map's "short
readable burst, brief lockout" is accurate; the numbers are not recorded.

**Audio is the highest-risk part of the port, and the identity most worth
protecting.** `music.js` is a hand-rolled WebAudio engine: a 70 ms-lookahead
scheduler over three themes stored as MIDI note arrays (menu 0.38 s beat, battle
0.205 s, victory 0.27 s), plus seven SFX built live from oscillators and filtered
noise with randomized pitch and gain per call. Godot has no WebAudio equivalent.
Two candidate approaches — `AudioStreamGenerator` (sample-level push, closest to
the original, least certain under Web export) or baking short `AudioStreamWAV`
buffers at load and varying `pitch_scale` per play (safe, covers most of the
required variation). This is not a late milestone. It should be spiked
immediately after the export gate, because it also answers the standing open
question of how much procedural audio the Web export can carry.

**One coupling to drop deliberately.** `music.js` triggers hurt / heal / pickup
SFX by attaching a `MutationObserver` to the HUD's DOM width. The feature map
already prescribes the replacement — signals emitted by combat, consumed by
audio — so this is a known improvement rather than a surprise.

**Deployment slot already exists.** `sneaker.games/games/plushy-guardian/` is a
launch wrapper page with title, controls, build notes and a feedback form; the
game itself is served from `/plushy/`. The "lightweight launch wrapper" listed as
a possible refinement in the feature map is already built. The Godot export
replaces `/plushy/` and leaves the wrapper alone.

**The existing tests will not transfer.** `plushy/tests/*.test.js` assert regexes
against the text of `index.html`. That works for a single-file canvas game and is
meaningless against a Godot scene tree. Do not port the pattern.

## Result

Proposed milestone plan. Not promoted to architecture.

**M0 — Export gate.** Move to the standard (non-.NET) Godot 4.7 build, drop the
`[dotnet]` section from `project.godot`, keep `gl_compatibility` (already
correct for web). Export the empty project, host it, open it on a phone. The
intent doc says web constraints are an architecture input from the start; this
is that promise made testable on day one.

**M1 — Audio spike.** Port `tone()` and `noise()` and get the menu theme looping
in a mobile browser with pitch-varied attack and dodge SFX, no crackle. Choose
between the generator and baked-buffer approaches on evidence. Record the
decision in architecture.

**M2 — Player controller.** `CharacterBody2D`; named input actions for move,
cast, dodge; speed 205; aim from last movement direction; dodge burst as above;
4-frame idle at 5 fps, flipped when aim.x < -0.2; wisp at 40 px.

**M3 — Combat components.** Hitbox and hurtbox as small `Area2D` components.
Signals for damaged, healed, died, impact. Vine projectile. One enemy — slime,
r23 / hp38 / speed60 / 11 contact damage — with its hop bob. Hurt response:
0.62 s invulnerability, 22 px knockback, screen shake, brief white flash, 20 Hz
blink.

**M4 — Level 1 encounter loop.** Three waves (5, then 4 + wave), the blighted
tree at hp 170 taking 12 per vine hit, seed pickups, stuffing heal (+9, 8 s
life). HUD as `Control` nodes: stuffing bar, level bar, objective line. This
completes the ten-point definition of success except touch and export.

**M5 — Touch and deploy.** Virtual stick and two action buttons driving the same
input actions; hidden on `pointer:fine` desktops; safe-area aware. Export, drop
into `sneaker.games/plushy/`, play it on a phone, keep the wrapper.

**M6 — Levels 2 and 3.** Bats, mushrooms, the per-level variants and palettes —
only once level 1 feels right in Godot. Reaching M6 is parity; everything after
it is chosen by playtest, per intent.

Sequencing rationale: M0 and M1 are risk gates and are cheap to fail. M2–M4 are
the vertical slice. M5 is the second risk gate — touch feel on a real phone.
M6 is breadth, and breadth is the thing most likely to be wrong to build early.

## Possible Architecture Changes

1. Record the ranged vine attack and implicit aim-by-movement in
   `CURRENT-DEMO-FEATURE-MAP.md` section 1. Currently absent.
2. Record the dodge, projectile, enemy and pickup numbers as the baseline the
   recreation is tuned against.
3. Label the ten-point definition of success in `current-state.md` as a subset of
   the shipped demo, with parity defined as the end of M6.
4. Note that the launch wrapper already exists, so it is no longer a refinement.
5. Add the audio approach decision once M1 produces evidence.

None applied. Promotion is deliberate.

## Open Questions

- Should the Godot version reproduce the demo's numbers exactly as a fidelity
  test, or re-tune from the start? The plan assumes exact first, re-tune after,
  because matching numbers makes "does it feel the same" answerable.
- The RLE sprite blobs in `plushy/art/sprites-*.js` need a one-time decode to
  PNG for Godot import. Trivial, but somebody has to do it — script or by hand.
- `guardian_idle.png` is a 4-frame 64 px sheet and the only player animation in
  the demo. Attack, dodge, hurt and heal have no dedicated frames; they are all
  procedural. Does the Godot version keep that, or is this where new art enters?

## Continuation

Awaiting a decision on the plan. If accepted, next action is M0: the export gate.
