---
name: staged-code-review
description: Review staged Git changes for actionable defects and regression risks using the index snapshot; keep the review read-only.
---

# Staged Code Review

Infer the problem and solution from the staged snapshot, verify concrete failure scenarios, and report actionable findings in severity order. Explicit user instructions take precedence over this guidance; preserve applicable project restrictions.

## Hard boundaries

- The target is `git diff --cached`. Unstaged and untracked changes are outside scope unless the user expands it; read them only as clearly labeled supplementary context when necessary.
- Do not modify files or mutate the index, working tree, branches, commits, or history.
- Do not run tests, coverage, formatters, linters, type checkers, builds, application code, or other commands with side effects.
- Do not install dependencies or access external systems unless separately requested.

These boundaries also apply when combining the review with a persona or design skill. Produce one report without duplicate findings.

## Establish the staged snapshot

Use index versions for changed files **and supporting callers, contracts, and tests**. Working-tree fixes must not hide staged defects, and unstaged defects must not become staged findings.

Start with an inventory and diff; disable optional locks and external diff or text-conversion helpers:

```bash
GIT_OPTIONAL_LOCKS=0 git status --short
git --no-pager diff --cached --no-ext-diff --no-textconv --name-status
git --no-pager diff --cached --no-ext-diff --no-textconv --find-renames
```

If nothing is staged, say so and stop. For unresolved merge entries, identify conflicts and limit conclusions about affected paths. Handle an initial commit without assuming `HEAD` exists.

For large diffs, read path-sized portions until all staged changes are accounted for; truncated output is incomplete. Use `--stat` for orientation and `--check` for whitespace or conflict markers when useful, never as a substitute for reviewing behavior.

Use targeted reads and searches with safely quoted paths and patterns:

```bash
git show ':path/to/file'
git show 'HEAD:path/to/file'
git grep --cached -n -e 'pattern' -- 'relevant/path'
git --no-pager diff --cached --no-ext-diff --no-textconv -- 'path/to/file'
```

For deletions and renames, compare the correct old path and cite old-side locations where needed; a staged file need not exist on disk. Assess binary metadata, submodule pointers, symlink targets, and generated-file relationships when relevant without executing or regenerating them. Disclose content that cannot be assessed.

Before finalizing, recheck the staged inventory and diff. If they changed, refresh affected evidence and locations. If changes continue, disclose the unstable snapshot instead of presenting mixed-version findings.

## Investigate concrete risks

Establish the intended behavior, affected contracts, realistic failure conditions, and whether staging is incomplete. Expand into nearby code only to confirm or falsify a concrete concern; do not audit the entire repository.

Prioritize:

1. Correctness: missing branches, nullability, ordering, partial updates, cleanup, and error paths.
2. Compatibility: callers, persisted data, schemas, public contracts, migrations, rollout, and rollback.
3. Security: authorization, input validation, injection, secrets, and sensitive output.
4. Reliability: transactions, races, async ordering, retries, idempotency, leaks, and recovery.
5. Performance: N+1 queries, unbounded work, blocking operations, allocations, and serialization.
6. Maintainability: ownership, indirection, hidden coupling, and established patterns.

Increase scrutiny around external input, authentication, persistence, concurrency, queues, caching, billing, permissions, and infrastructure. Report only issues introduced or materially worsened by the staged changes.

Inspect staged tests without executing them. Assess focused, deterministic behavioral coverage and identify precise missing scenarios and the best existing test location. Missing tests alone do not prove broken behavior; report a coverage finding only for a meaningful demonstrated regression risk and avoid duplicating a defect as another coverage finding.

## Findings and response

Check actual callers, guards, and tests before reporting. Drop disproved concerns; missing context is a limitation, not evidence of a defect. Consolidate findings sharing a root cause and keep unresolved questions separate.

Use project severity conventions, otherwise:

- **P0:** unconditional critical release blocker.
- **P1:** concrete serious failure needing prompt correction.
- **P2:** actionable defect or regression risk under a realistic condition.
- **P3:** localized issue with demonstrated impact; omit taste-only polish.

Each finding needs a concise severity title, a precise staged file and line or smallest relevant range, the triggering scenario, the consequence, and the smallest viable correction when clear. Mark old-side locations explicitly. Severity reflects impact and reachability, not confidence.

Lead with findings in severity order. If none are supported, explicitly say no actionable staged-change findings were found. Briefly add inferred intent, important tradeoffs, relevant coverage or staging gaps, and verification limits only when useful. State that tests were not run. Suggested commands must be narrow and clearly unexecuted; never claim fixes were applied. A clean review is a valid result, not proof of correctness.
