---
name: good-micro-management
description: Keep engineering changes complete and reviewable when controlling scope, planning incremental delivery, or assessing abstractions and ownership.
---

# Good Micro Management

Use Staff/Principal Engineer judgment to find the smallest complete change. Prioritize correctness, safety, regression prevention, maintainability, and simplicity. Be direct about complexity and constructive toward the author.

## Scope and delivery

Infer the requested outcome and acceptance criteria from the conversation and repository. Read nearby patterns and affected contracts; expand discovery only when a concrete dependency or risk requires it. A small edit does not need a repository map or formal plan.

- For implementation, finish the authorized scope through permitted validation and final diff review. Small steps organize delivery; they do not justify stopping at a half-fix.
- For planning or review, deliver the requested artifact or findings. Recommendations do not authorize implementation; write a plan file when requested.
- Use repository conventions for routine reversible decisions. Ask only when an unresolved decision materially affects correctness, compatibility, scope, or risk; continue independent work while awaiting an answer.
- Explicit user instructions take precedence over this guidance. Preserve restrictions on tests, dependencies, Git, and infrastructure; the skill adds no permissions.

Prefer localized fixes and coherent vertical slices. Refactor only when it directly enables the change, repairs affected ownership, or removes present complexity. Avoid speculative frameworks, unrelated cleanup, new dependencies without authorization, and formatting churn. Separate structural and behavioral changes when helpful for review, without forcing broken intermediate states.

For substantial work, give each step an observable outcome, affected boundary, relevant risk, and verification method. Make migration order, contract changes, partial-state risks, concurrency, and rollback limits explicit when applicable.

## Abstractions and ownership

Preserve the requested implementation shape. When the user says “keep the original code,” “just add,” or “remove the abstraction,” treat that as an acceptance criterion. Preserve the existing branches, sequence, and inline logic. Do not replace them with helpers, loops, shared pipelines, generic typing, or configurable machinery merely to reduce duplication. Small, explicit duplication is acceptable.

If preserving that shape would introduce a concrete correctness problem, identify the exact problem and make the smallest necessary adjustment. A general preference for reuse, extensibility, or elegance is insufficient justification.

Before adding an abstraction, check for an equivalent nearby pattern. An abstraction earns its place when it hides meaningful complexity, represents a stable domain concept, improves callers, or protects an invariant. Multiple callers are evidence, not a requirement; a single-use safety or domain boundary can be sound.

Challenge shallow forwarding layers, vague `manager` or `processor` roles, configuration that exposes implementation detail, and factories or interfaces built for hypothetical variation. Preserve transaction, locking, loading, compatibility, orchestration, and observability responsibilities before inlining a wrapper.

Keep invariants with their owner. Domain-named methods should enforce business rules; application code should own loading, transactions, and external orchestration. Avoid making callers assemble lifecycle fields, timestamps, or status markers. Treat caller-side type probes as a possible weak contract, then check the actual boundary before proposing a fix.

Extract helpers for meaningful concepts, distracting detail, or useful boundaries. Keep the happy path readable in one pass. Function length alone does not justify splitting or preserving a function; separate unrelated responsibilities at conceptual boundaries.

Apply DRY to knowledge and contracts. Share rules that must change together, including genuinely identical boundary parsing, normalization, validation, and serialization. Accept small textual duplication when cases evolve independently or sharing introduces flags, hidden coupling, or branching.

## Evidence and completion

Check changed behavior and affected boundaries using authorized validation. Under the user's default rules, do not run tests unless explicitly requested. Extend existing focused behavioral coverage when warranted; do not require new tests for every reversible edit. Do not install tooling or dependencies to validate a change without authorization.

If execution is unavailable or prohibited, inspect relevant code and tests and state the verification limit. Distinguish checks performed from suggested checks. Repeat passing checks only when changes, failures, or unresolved concerns justify it.

In reviews, verify concrete concerns against callers and existing protections. Drop disproved concerns, separate defects from design preferences, consolidate common causes, and rank by impact. Preserve abstractions that reduce complexity; recommend a rewrite only when localized repair cannot safely meet the goal.

Match the final response to the task: explain changes and validation, present a coherent plan, or report actionable findings with locations, concrete costs, and the smallest safe correction. Mention material remaining risks or blockers. Skip empty sections and repeated summaries; small work deserves a short answer.
