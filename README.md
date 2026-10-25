# .github

Org-wide defaults for `Travelin-ai` repos.

- `PR_CONVENTIONS.md` — the size budget, PR stack, stream label,
  stacked-base, and review-test rules every repo follows.
- `PULL_REQUEST_TEMPLATE.md` — GitHub applies it automatically to any
  repo in the org that has no PR template of its own.

## Opting in to the PR size check

`pr-size.yml` is a reusable workflow, not applied automatically. A
repo opts in with a caller workflow like:

```yaml
name: PR size
on:
  pull_request:
    types: [opened, reopened, ready_for_review]
permissions:
  pull-requests: write
jobs:
  size:
    uses: Travelin-ai/.github/.github/workflows/pr-size.yml@main
```

Not on `synchronize`: every run is billed a full minute, so the size is
counted when the PR opens and again when a draft is marked ready, not on
every push. The check is warn-only, so a stale label between pushes costs
nothing.

The `trav` Claude Code plugin carries a mirror of `PR_CONVENTIONS.md`; change this file first, then the mirror.
