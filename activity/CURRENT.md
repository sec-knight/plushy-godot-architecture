# Current

**Last session:** `activity/2026-08-13-architecture-scaffold.md`

## Where things stand

The architecture repository now follows the Grimoire protocol: local
instructions, canonical `architecture/`, and an `activity/` trail. Design is
captured — intent, the demo baseline in `CURRENT-DEMO-FEATURE-MAP.md`, and
current state. Implementation has not started; `plushy-godot-source` is still a
bare Godot seed with no scenes or scripts.

## Next action

Begin milestone 1 in `plushy-godot-source`: a `CharacterBody2D` player driven by
named Godot input actions, with move, attack, and dodge.

## Do not retry

- SSH remotes (`git@github.com:sec-knight/...`) from the current Windows machine
  — no usable key, fails with `Permission denied (publickey)`. Use the HTTPS
  remotes instead.
