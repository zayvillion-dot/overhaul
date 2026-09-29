# Quality bar, rationalization table, and extended red flags

Reference material for `../SKILL.md` — moved out here to keep the top-level
contract short. Nothing here is optional; it's just not inline.

## Quality bar

- Click-speed / time-to-complete, not just correctness: a fix that works but adds a tap or buries information isn't done.
- Simplicity without losing information — condensing a finding or a stats block is not the same as deleting what someone still needs.
- Scope = organization, testing, functionality, and fixes of existing code. **Never mass deletion** — an overhaul repairs a running system, it doesn't gut it.
- When a reviewed sentence turns out false, delete it — never rewrite it smaller and leave a shrunken false claim standing.

## Rationalization table

| Excuse | Reality |
|---|---|
| "It's quick, just fix it now" | Skipping the register loses the cross-cutting map; today's off-register quick fix is tomorrow's untracked regression. |
| "Small enough to skip the worktree" | A shared file between two live legs is exactly how one agent's edit erases another's. |
| "It's cosmetic, skip jedi-review" | Every target gets a tribunal sized per jedi-review's own stage 1 Scope — never zero lenses. |
| "Review the whole batch at the end" | Waiting until the register is empty means every leg's merge conflicts and regressions land at once instead of one leg at a time. |
| "I can already see what's wrong, skip the probe" | A guessed fix that misses becomes a second, harder-to-see defect stacked on the first. |
| "Fold the console steps into one paragraph" | A step buried in prose is a step that gets re-parsed wrong; per-item WHAT/WHERE/HOW is what makes it executable without a follow-up question. |
| "It's just a comment, it can say anything" | A comment sharing a file with a substring test can silently satisfy or break that test. |

## Extended red flags

See `SKILL.md`'s Red flags for the core list. These belong to the same "stop and back up" rule:

- A guessed fix shipped for a symptom no explorer confirmed the cause of.
- Findings sitting in chat history instead of the register because "I'll add them later."
- A batch gate launched without checking for a foreign suite or an in-flight push first.
- A merge across an auto-mergeable shared file with no merge-collision lens run against it.
