# Overhaul

A Claude Code skill for a live human test-pass. A live test pass produces findings faster than anyone can fix them; the register is the buffer between the two. Log a finding the instant it lands, keep the human testing, map causes before assigning fixes, and batch reviewed legs into deploys the human can re-test live — instead of alternating "find one, stop, fix one."

Use it when the user is running a live human test-pass (mobile, browser, device) and feeding in findings one at a time while testing continues, invokes `/overhaul` or says "let's overhaul this," or hands over a raw batch of bug/UX findings from a test round. Not for a single isolated bug report, a first-time build, or steady-state one-off fixes.

## Requires

The [jedi-review](https://github.com/zayvillion-dot/jedi-review) skill. Every leg ends in a jedi-review pass before it can merge.

## Install

**As a plugin:**

```
/plugin marketplace add zayvillion-dot/overhaul
/plugin install overhaul@overhaul
```

**Manual:**

Copy `skills/overhaul/` into `~/.claude/skills/`.

## License

MIT — see `LICENSE`.
