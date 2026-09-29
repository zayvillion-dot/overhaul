# Wave / batch runbook

Mechanics for stage 3 (waves) and stage 6 (batch & deploy) — the parts too procedural to keep inline in `SKILL.md`. Worked shapes below are patterns, not literal scripts to copy verbatim; adapt the commands to the target repo's own test entrypoint and deploy tooling.

## Wave dispatch

1. Cap: **≤5 builder legs concurrently, session-wide** — count every leg currently building in its own worktree; review lenses and skeptics are jedi-review's business and are not counted, and neither is the merge-collision lens. A wave that would push the session's builder total past 5 splits: the overflow legs queue for the next wave.
2. Every leg dispatch cites its wave and its ownership-table row.

## Pre-flight (run before every batch gate, not just the first)

Check all five before launching a full-suite run on the integration branch:

- **Clean tree:** the main working copy has no uncommitted changes.
- **In sync:** `git log origin/main..main` is empty — local `main` is not ahead of the remote in a way this gate doesn't know about.
- **Integration ⊇ origin/main:** `git merge-base --is-ancestor origin/main <integration-tip>` is true. `main == origin/main` only checks the local `main` branch — it says nothing about the integration tip that is about to be gated and pushed. If this check is false, merge the missing commits into the integration branch, re-run the merge-collision lens on anything they touched, and re-check before gating.
- **No push in flight:** `pgrep -f "[g]it push origin main"` comes back empty.
- **No foreign suite:** `pgrep -f <the project's suite entrypoint>` comes back empty — a second session's own test run can silently race or kill this one.

If any check fails, resolve it — do not launch the gate speculatively "to see what happens." It never means pushing or resetting another session's work: **an unpushed local `main` you did not create is not yours to push or reset** — even when it fails the "in sync" check — wait for that session's own push, then merge `origin/main` in as usual.

## Deploy

1. Push/deploy via the project's own deploy tooling; capture its log.
2. Verify the live site/app reflects the change (a version marker, a specific page, a specific behavior) — a deploy with no visible change (e.g. no static asset touched) needs its own verification method; don't rely on a build-id stamp that this batch never actually moved.
3. Remove the leg worktrees once merged and deployed; keep the branches unless the user says otherwise.
