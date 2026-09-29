# Merge-collision lens

An auto-merge that resolves cleanly at the text level can still silently drop a line, or let two independently-correct changes combine into a behavior neither side intended.

This is not jedi-review's fix-blast-radius lens (that one attacks a single fix's reach). This lens attacks the *merge itself* — the act of combining two deltas into one file.

## What it proves

For every auto-merged shared file, prove — in both directions — that the merged file contains the exact union of both sides' deltas:

1. Diff the **merge base** (`git merge-base` of the two sides, not the integration branch's pre-merge tip) against each side's post-change version independently. For a leg vs. a moving `origin/main`, the integration branch's own pre-merge version already contains that leg's delta and is not the common base — diffing against it makes the leg's side of the proof vacuous.
2. Diff the merged file against each side's post-change version.
3. Every line either side added is present in the merged file (no lost-line).
4. No line survives in the merged file that either side had deleted (no resurrected-line).
5. Where both sides touched the *same* lines (a true text conflict, not just a nearby-lines coincidence), read the resolution as its own small finding — a clean text merge can still combine two changes into a broken behavior.

## Worked example (from a real session)

Two changes landed on the same paper-rendering file: one gated a discount label on `discount_reason` for five existing customer-facing renderers; a second change added a sixth renderer (a flattened snapshot) reading the discount flag alone, with no `discount_reason` check. The merge was textually clean — no conflict markers, nothing lost — and still wrong: a reason-less discount now printed on the sixth renderer's paper while the other five correctly refused it.

Fix pattern: pull the gating predicate into one shared function all six call sites use, so the sixth reader can't silently diverge from the other five's rule.

## Adjudication

This lens is a jedi-review lens, not a self-graded check: its findings go through jedi-review's own stage 3 (Adjudicate) — a fresh skeptic, never this lens, returns CONFIRMED or REFUTED — and its confirmed findings go to that project's `LESSONS-DB.md` per jedi-review's stage 6. Do not report a verdict here without that adjudication step.

## Report format

A finding here is fixed as its own small leg (single-owner file, jedi-reviewed) before the batch gate runs — it is not patched inline on the integration branch.
