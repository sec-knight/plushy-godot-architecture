# Plushy Godot — Current Demo Feature Map

## Purpose

This document captures the behavior and feel of the existing Plushy browser demo as the baseline for the Godot recreation. The goal is not a literal port. Each feature should either be preserved, improved, or deliberately replaced in Godot while keeping the demo's playful plush-toy identity.

## Baseline: current Plushy demo

### Player character
- A plush mascot is the playable character.
- Movement is immediate and arcade-like rather than simulation-heavy.
- The character supports combat, dodging, taking damage, and healing.
- The mascot's presentation is intentionally soft, silly, and expressive rather than realistic.

### Combat feel
- The player can attack enemies.
- Attacks use lightweight, readable effects rather than elaborate simulation.
- Dodging is part of the moment-to-moment movement/combat loop.
- Damage and healing have their own feedback cues.

### Audio identity
- The demo has a deliberately simple, playful background track built from small "beep / boop / doot" sounds.
- The intended mood is closer to a cozy starting-area game soundtrack than cinematic music.
- Sound effects use small procedural variations so repeated actions do not sound identical.
- Current sound vocabulary includes attack/impact whooshes, dodge sounds, footstep bops, hurt cues, healing purrs/pops, and related tiny synthetic effects.

### Presentation
- The demo is designed around the Plushy mascot as the central visual identity.
- Feedback is intentionally exaggerated enough to feel playful and readable.
- The prototype favors charm and responsiveness over visual complexity.

### Web-first usage
- The game is intended to be playable directly from sneaker.games.
- Mobile and tablet play are important because the demo should be easy to share and play with family.
- Touch controls are therefore a first-class target rather than a late compatibility feature.

## Godot recreation targets

### 1. Player controller

**Preserve**
- Fast, simple movement.
- Easy-to-read attack and dodge behavior.
- Low friction between input and response.

**Godot approach**
- Use `CharacterBody2D` for the player.
- Define movement, attack, dodge, and interaction through Godot input actions rather than device-specific keys.
- Keep the controller input-source agnostic so keyboard, gamepad, and touch can drive the same actions.
- Use a small explicit player state machine for movement / attacking / dodging / hurt / defeated states if the behavior becomes difficult to reason about with simple flags.

**Possible refinement**
- Add acceleration/deceleration only if it improves the plushy feel; default to responsiveness over realism.
- Give dodge a short readable burst, brief lockout, and clear audiovisual cue instead of making it a complex stamina mechanic.

### 2. Combat

**Preserve**
- Simple attacks.
- Clear enemy/player hit feedback.
- Hurt and healing readability.

**Godot approach**
- Separate hit detection from presentation using small hitbox/hurtbox components.
- Emit signals/events for damage, healing, death, and impacts so audio, particles, animation, UI, and game logic do not become tightly coupled.
- Start with one primary attack and one dodge before adding additional abilities.

**Possible refinement**
- Add hit-stop, tiny knockback, squash/stretch, particles, or screen shake in very small amounts to make contact satisfying.
- Keep combat readable enough for a phone-sized viewport.

### 3. Animation and mascot expression

**Preserve**
- Plushy should feel soft, goofy, and alive.

**Godot approach**
- Use `AnimationPlayer`, `AnimatedSprite2D`, or both depending on final asset format.
- Treat idle, walk, attack, dodge, hurt, heal, and defeat as the first animation set.
- Where full sprite animation is unnecessary, use procedural secondary motion such as bounce, squash/stretch, tilt, and recovery.

**Possible refinement**
- Give Plushy subtle idle variation so the character does not look frozen when standing still.
- Use animation events to synchronize footsteps, attack impacts, and other procedural sounds.

### 4. Audio system

**Preserve**
- Cozy beep-boop background identity.
- Small synthetic effects.
- Procedural variation between repeated sounds.

**Godot approach**
- Build a reusable audio service or scene responsible for one-shot effects and music.
- Create families of related sounds rather than one fixed sample per action.
- Randomize safe parameters such as pitch, sample choice, timing offset, or envelope within intentionally narrow ranges.
- Keep music and SFX buses separate so volume can be controlled independently.

**Possible refinement**
- Recreate the background music as a procedural or semi-procedural looping system in Godot instead of relying on one baked track, if browser performance remains acceptable.
- Let footsteps vary by movement rhythm and eventually by surface type.
- Give healing, hurt, dodge, and attacks their own recognizable sonic signatures.

### 5. Enemies and encounter loop

**Preserve**
- Lightweight enemies that create something immediate for Plushy to fight or avoid.

**Godot approach**
- Use a small reusable enemy scene with health, movement/targeting, hitbox/hurtbox, and presentation components.
- Prefer composition over building a deep enemy inheritance hierarchy for this demo.

**Possible refinement**
- Introduce 2–3 strongly differentiated enemy behaviors rather than many shallow variants.
- Keep encounter density suitable for both desktop and touch input.

### 6. UI and feedback

**Preserve**
- Immediate visual understanding of health and important game state.

**Godot approach**
- Build UI with `Control` nodes and anchors so the same HUD can adapt across desktop, iPhone, and iPad aspect ratios.
- Keep gameplay UI separate from the world scene.

**Possible refinement**
- Make the HUD deliberately toy-like and visually consistent with Plushy rather than using generic game UI.
- Allow touch controls to disappear automatically when a non-touch input method is active where practical.

### 7. Touch and mobile web controls

**Target from the beginning**
- The web build should remain comfortably playable on phones and tablets.

**Godot approach**
- Implement virtual movement plus attack/dodge controls through the same action layer used by desktop controls.
- Respect safe areas and different aspect ratios.
- Avoid tiny buttons and precision interactions.
- Test iPhone/iPad browser behavior during development rather than only at release.

**Possible refinement**
- Support drag-based movement or a virtual stick after comparing which feels better for the game.
- Make touch controls visually quiet when not being used.

### 8. Web deployment

**Target**
- Export a Godot Web build that can live as a playable experience on sneaker.games.

**Godot approach**
- Keep web-export constraints visible in architecture decisions from the start.
- Treat exported files as build artifacts rather than architecture knowledge.
- Automate deployment only after a manual export/deploy loop is understood and reliable.

**Possible refinement**
- Give the site a lightweight launch wrapper with game title, version, controls, changelog/devlog link, and feedback entry point.

### 9. Feedback and iteration

**Target**
- Use this project to test a complete Grimoire development loop, not merely produce a game build.

**Godot / project approach**
- Record meaningful playtest feedback as project evidence.
- Convert accepted feedback into explicit design decisions or work items.
- Preserve screenshots, superseded builds, raw feedback, and other bulky historical evidence in the archive when they no longer belong in active architecture.
- Keep durable conclusions and current design state in the architecture repository.

## Initial definition of success

The first Godot milestone should reproduce the recognizable core of the current browser demo:

1. Plushy can move.
2. Plushy can attack and dodge.
3. At least one enemy can interact with the player through damage/defeat rules.
4. Hurt and healing feedback work.
5. The playful procedural SFX identity is present.
6. A simple beep-boop background track is present.
7. Keyboard controls work.
8. Touch controls work on a phone-sized viewport.
9. The project exports successfully to Godot Web.
10. The build can be hosted and launched from sneaker.games.

After that baseline is reproduced, improvements should be chosen through playtesting rather than by expanding scope speculatively.

## Open questions to resolve through the pilot

- Which parts of the existing browser demo are genuinely fun enough to preserve exactly?
- Which behaviors feel materially better when implemented with Godot's animation, physics, audio, and scene systems?
- Should the final visual style remain sprite-based, use richer 2D effects, or move toward a 2.5D treatment?
- What touch-control scheme feels best on iPhone and iPad?
- How much procedural audio can the Web export comfortably support?
- What feedback is worth promoting into durable architecture versus relegating to the archive?
- What information does Grimoire need at `/resume` for a new AI session to continue this project without rediscovering its design?
