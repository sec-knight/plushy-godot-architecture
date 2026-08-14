# Current

**Last session:** `activity/2026-08-13-architecture-scaffold.md`

## Where things stand

The architecture repository now follows the Grimoire protocol: local
instructions, canonical `architecture/`, and an `activity/` trail. Design is
captured — intent, the demo baseline in `CURRENT-DEMO-FEATURE-MAP.md`, and
current state. Implementation has not started; `plushy-godot-source` is still a
bare Godot seed with no scenes or scripts.

A routing defect was diagnosed and recorded in
`black-clover/findings/2026-08-13-ssh-remotes-unusable.md`. The fix is decided
but not applied, so the SSH workaround below still applies.

## Next action

Begin milestone 1 in `plushy-godot-source`: a `CharacterBody2D` player driven by
named Godot input actions, with move, attack, and dodge.

## Do not retry

- **SSH remotes** (`git@github.com:sec-knight/...`). GitHub SSH was never
  configured on this Windows machine — `~/.ssh/config` defines only the retired
  `wizard` / `familiar` hosts. Use the HTTPS remotes; they are authenticated and
  every local repository already uses them.
- **Generating a GitHub SSH key** to make the map work as written. No SSH
  consumers remain to justify a second auth path.
- **Recording both URL forms** in `projects.yaml`. Pushes the decision onto every
  reader for no benefit.
