# Overhaul

A Claude Code skill for a live human test-pass. A live test pass produces findings faster than anyone can fix them; the register is the buffer between the two. Log a finding the instant it lands, keep the human testing, map causes before assigning fixes, and batch reviewed legs into deploys the human can re-test live — instead of alternating "find one, stop, fix one."

Use it when the user is running a live human test-pass (mobile, browser, device) and feeding in findings one at a time while testing continues, invokes `/overhaul` or says "let's overhaul this," or hands over a raw batch of bug/UX findings from a test round. Not for a single isolated bug report, a first-time build, or steady-state one-off fixes.

## Requires

The [jedi-review](https://github.com/zayvillion-dot/jedi-review) skill. Every leg ends in a jedi-review pass before it can merge.

## Install

**As a plugin:**

```
/plugin marketplace add zayvillion-dot/jedi-review
/plugin install jedi-review@jedi-review
/plugin marketplace add zayvillion-dot/overhaul
/plugin install overhaul@overhaul
```

**Manual:**

Copy `skills/overhaul/` into `~/.claude/skills/`.

## Known gaps in 0.1.0

This version was reduced to the rules that survived review. It does not yet cover the following; use your own judgment, and ask the person you are working for before any step that could lose work or reach the trunk.

1. Nothing says how the gated tip reaches the trunk, requires the pushed commit to be the gated commit, or forbids a force-push.
2. No step runs `git fetch`, so every check against `origin/main` can pass on stale data.
3. The skill does not say what to do with uncommitted changes the agent did not make, or where the integration branch is checked out, so the clean-tree check may look at the wrong tree.
4. The register lives outside the repo, no branch is backed up until a deploy, and there is no way to resume an interrupted session.
5. There is no stated way to detect another session in the same repo, or to tell whose commits or changes are whose.
6. The integration branch is cut "off main" without saying local or remote, so another session's unpushed commits can ride along.
7. Nobody is named to resolve a real merge conflict, and the skill does not say whether the merge is committed before the lens runs.
8. The merge-collision lens checks for lost and resurrected lines only; it does not check that two clean changes still behave correctly together, and it does not look across files.
9. A failed post-deploy check, or a red CI after the push has landed, has no rollback or next step.
10. Overhaul merges a passed leg into the integration branch without asking, while jedi-review says merge is the user's call.
11. Several reviews run at once and all write `LESSONS-DB.md`; no writer or tree is named.
12. A leg that stops without a PASS has no status, keeps its file ownership, and blocks legs queued behind it.
13. Removing a worktree with leftover changes invites `--force`; stray writes in the main copy have no remedy.
14. There is no session close: no end condition and no stated fate for unmerged branches.
15. A "probe" is never defined, including whether it is code that ships and when it is removed.
16. What counts as a standing authorization to deploy is not defined.
17. The red-suite fix cycle has no limit, and backing a merge out is not addressed.
18. The push-in-flight and foreign-suite checks are narrow patterns; the second also matches this session's own leg tests.
19. Who fixes review findings, who creates worktrees, how the lens is run and by whom, and which two sides the merge base uses are all unstated.

## License

MIT — see `LICENSE`.
