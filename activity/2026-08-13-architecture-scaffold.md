# Session: Stand up Grimoire structure; diagnose SSH routing defect

**Date:** 2026-08-13
**Environment:** Claude Code

## Goal

Make the plushy-godot architecture repository conform to the Grimoire protocol so
a later session can resume the project without depending on this conversation.

Second half of the session turned to the SSH remote failure found along the way,
which proved to be a routing-map defect rather than a plushy-godot one.

## Context Used

- Personal Grimoire (`black-clover`): `README.md`, `GRIMOIRE.md`, `AGENTS.md`,
  `CHAT.md`, `projects.yaml`, and `prompts/session-open.md`,
  `prompts/session-close.md`, `.agents/skills/{resume,endsession}/SKILL.md`.
- `grimoire/templates/architecture-repo/` — the canonical template, found locally.
- `plushy-godot-architecture` at commit `4757e90`.
- `plushy-godot-source` at commit `0644c04`.

## Work

- Resolved `plushy-godot` through `projects.yaml` to its three repositories.
- Cloned `plushy-godot-architecture` to
  `C:/Users/Administrator/Documents/GitHub/plushy-godot-architecture` over HTTPS.
- Copied the canonical template verbatim: `AGENTS.md`, `FAMILIAR.md`,
  `architecture/README.md`, `activity/README.md`, `activity/SESSION-TEMPLATE.md`.
- Moved `CURRENT-DEMO-FEATURE-MAP.md` from the repository root into
  `architecture/` with `git mv`, so it is picked up by the protocol's
  "read `architecture/` first" step.
- Wrote `architecture/intent.md` and `architecture/current-state.md` from the
  feature map and the observed state of both repositories.
- Wrote this record and `activity/CURRENT.md`.
- Committed in two parts per protocol — `d248b69` `architecture:` and `9ba9405`
  `activity:` — then `298f710` correcting this record's own commit state. Pushed.

### SSH investigation — work landed in `black-clover`

- Added `findings/` to the personal Grimoire with a `README.md` scoping it to
  routing and access infrastructure only, since `black-clover/AGENTS.md` warns
  against that repository growing.
- Wrote `findings/2026-08-13-ssh-remotes-unusable.md`, then revised it once the
  diagnosis sharpened. Commits `b3b2e19` and `2f6e28c`, both pushed.
- Enumerated every SSH reference across the local repositories and `~/.ssh`.

## Findings

- The architecture repository had none of the structure the Grimoire prompts
  assume — no `AGENTS.md`, no `architecture/`, no `activity/`. A session opened
  with `resume` would have found the route valid but the destination empty.
- A canonical `architecture-repo` template already exists in the local `grimoire`
  repository. It was used verbatim rather than reinvented. Worth knowing that
  `grimoire` is the source of protocol templates, separate from `black-clover`,
  which is the personal routing map.
- The SSH remotes in `projects.yaml` (`git@github.com:sec-knight/...`) fail from
  this machine: `Permission denied (publickey)`. HTTPS to the same repositories
  works, and `plushy-godot-source` was already configured with an HTTPS remote.
- `plushy-godot-source` is a bare Godot seed: no scenes, scripts, input actions,
  or assets. Milestone 1 is entirely unstarted.
- The archive repository was not cloned or inspected this session.

### On the SSH failure — first diagnosis was wrong

- Initially reported as "no usable key." That was incorrect. The real cause:
  `~/.ssh/config` defines only `wizard` and `familiar`, and the sole key
  `familiar_codex_sprints` is pinned to the `familiar` alias with
  `IdentitiesOnly yes`. No `Host github.com` entry, no default-named key.
  GitHub SSH was never configured here — nothing is revoked or broken.
- Root cause is upstream of `black-clover`: the SSH URL pattern comes from
  `grimoire/templates/personal-grimoire/projects.yaml`. Fixing only the map
  reintroduces the defect at the next seeding. Note that `grimoire` is the
  protocol/template repository, distinct from `black-clover`, the routing map.
- The `wizard` / `familiar` hosts are leftovers from the earlier
  context-management project; that server is retired and documentation now flows
  through GitHub. All seven local repositories already use HTTPS origins, and no
  `url.*.insteadOf` rewrites exist. `projects.yaml` is the only artifact on the
  machine still asserting SSH.
- `known_hosts` contains `familiar.taila1d25f.ts.net`. The leftover key may still
  be a live credential to that Tailscale node, so `~/.ssh` cleanup is not
  obviously safe.

## Result

The architecture repository now satisfies the protocol. `resume` against
`plushy-godot` will find local instructions, canonical architecture, a current
pointer, and one dated activity record.

Verification state: file structure confirmed on disk against the template.
Committed as `d248b69` (architecture) and `9ba9405` (activity), pushed to
`origin/main`.

## Possible Architecture Changes

Not promoted; recorded for deliberate decision later.

- The SSH-vs-HTTPS gap belongs in the personal Grimoire, not here — resolved
  during the session by adding `black-clover/findings/`. The note in
  `architecture/current-state.md` is now a duplicate pointer and can be trimmed
  to a reference once the underlying issue is fixed.
- `architecture/decisions.md` is not yet warranted — no decision has been
  superseded. Create it at the first real reversal, not before.

## Open Questions

- Should the browser demo's source and assets be brought into the archive
  repository so the behavioral baseline survives independently of the running
  demo?
- What does the archive repository currently contain?
- Is the `familiar` Tailscale node still running, and is
  `familiar_codex_sprints` still a live credential to it?

## Continuation

**Project:** plushy-godot
**Architecture:** `sec-knight/plushy-godot-architecture` (local clone at
`C:/Users/Administrator/Documents/GitHub/plushy-godot-architecture`)
**State:** Scaffold committed and pushed to `origin/main`. SSH defect diagnosed
and recorded in `black-clover/findings/`; the fix is decided but **not applied**.
**Next action:** Begin milestone 1 in `plushy-godot-source` with the
`CharacterBody2D` player and input actions.

**Pending, outside this repository** — rewrite the three SSH URLs in
`black-clover/projects.yaml` and the five in
`grimoire/templates/personal-grimoire/projects.yaml` to HTTPS. Then clear the
stale SSH notes in `architecture/current-state.md` and `activity/CURRENT.md`.

**Ruled out:** SSH remotes from this machine. Generating a GitHub SSH key —
no remaining SSH consumers to justify a second auth path. Recording both URL
forms in the map — hedges for an audience that does not exist.
