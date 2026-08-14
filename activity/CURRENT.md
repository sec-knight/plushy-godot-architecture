# Current

**Last session:** `activity/2026-08-13-implementation-plan.md`

## Where things stand

The architecture repository now follows the Grimoire protocol: local
instructions, canonical `architecture/`, and an `activity/` trail. Design is
captured — intent, the demo baseline in `CURRENT-DEMO-FEATURE-MAP.md`, and
current state. Implementation has not started; `plushy-godot-source` is still a
bare Godot seed with no scenes or scripts.

A routing defect was diagnosed and recorded in
`black-clover/findings/2026-08-13-ssh-remotes-unusable.md`, and has since been
fixed: `projects.yaml` and the Grimoire template now record HTTPS URLs only. No
workaround is needed when following the map.

The shipped demo source was read for the first time this session, from
`sneaker.games/plushy/`. It is materially further along than
`CURRENT-DEMO-FEATURE-MAP.md` describes — three levels, three enemy kinds with
variants, a boss phase, pickups. The feature map was written from memory of the
demo, not from its source. See `activity/2026-08-13-implementation-plan.md`.

## Next action

M0, the export gate: move to the standard (non-.NET) Godot 4.7 build, drop the
`[dotnet]` section from `project.godot`, and export the empty project to web and
open it on a phone before any gameplay work. The player controller is M2, not
first — M0 and M1 (the audio spike) are risk gates and are cheap to fail.

## Do not retry

- **SSH remotes** (`git@github.com:sec-knight/...`). GitHub SSH was never
  configured on this Windows machine — `~/.ssh/config` defines only the retired
  `wizard` / `familiar` hosts. `projects.yaml` no longer records SSH URLs, so
  this should not resurface; if an SSH URL appears anywhere, it is stale.
- **Generating a GitHub SSH key.** No SSH consumers remain to justify a second
  auth path.
- **Recording both URL forms** in `projects.yaml`. Pushes the decision onto every
  reader for no benefit.
