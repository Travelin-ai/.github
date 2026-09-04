# PR conventions

These apply to every Travelin-ai repo. A mirror of this file lives in
the `trav` Claude Code plugin — change here first, then update the
mirror.

## 1. PR size budget

Aim for about 600 source lines and about 1,500 total lines per PR,
counted as additions + deletions. "Source" excludes tests, docs,
snapshots, and generated files. This is a guideline, not a gate: when
a PR runs over, split it by work package rather than trimming tests.
A 4,000-line PR is not reviewable by a human even when two-thirds of
it is tests.

## 2. Plans name their PR stack

Every implementation plan lists the PRs it will produce, each with an
estimated source-line count, and states the merge order. One PR is
fine when the whole change fits the budget.

## 3. Stream label

Related PRs get ONE label, `feature:<slug>` (or `integration:<slug>`
for third-party integrations), mirrored in every repo the stream
touches — backend `Travelin-ai/travelin`, frontend
`Travelin-ai/travelin-frontend`. Create on first use, apply with:

```
gh label create "feature:<slug>" --repo Travelin-ai/<repo> --color 5319E7 --description "<≤100 chars naming the spec issue>"
gh pr edit N --add-label feature:<slug>
```

Don't add per-type labels (backend/frontend — the repo already says
that) and don't open an epic issue for a short stream. A "Stacked on
#N" line in the PR body still carries merge order; a label can't
express order. Existing examples: `feature:admin-user-delete`,
`feature:multi-region`, `integration:sabre-seat-selection`.

## 4. Stacked-base hygiene

Before opening a stacked PR, merge (or rebase) `origin/master` into
the base branch and push it, so the compare view shows only the new
package. Retarget the PR to master the moment its base merges. Check
`gh pr view N --json additions,deletions` before un-drafting.

GitHub pins a PR's base SHA at creation and doesn't recompute
additions/deletions when the base branch is later pushed. Force a
recount with:

```
gh pr edit N --base master && gh pr edit N --base <stack-base>
```

This recomputes within seconds and makes no history change.

## 5. Review-round test discipline

Tag reviewer test asks as **must** (proves a P0–P2 fix) or **nice**
(extra assertions, harness rewrites); nice goes to a follow-up PR on
the stack. Helper behaviour is proven once, in the helper's own
suite — per-path suites assert wiring only. Shared test doubles ship
as their own small PR first.
