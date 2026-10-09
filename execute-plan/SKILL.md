---
name: execute-plan
description: Execute or resume an existing software implementation plan and record progress against its outcomes.
---

# Execute plan

Before starting, read and follow [good-micro-management](../good-micro-management/SKILL.md) unless its full instructions are already loaded in this conversation. Apply its guidance throughout this workflow, within the user's authorized scope and applicable project restrictions. If unavailable, report that limitation rather than silently skipping it.

Read the specified plan, or `PLAN.md`, plus applicable project instructions and relevant code. Check completed items against current evidence before repeating work. Identify the next actionable item and verify that it still fits the codebase. Split oversized items into coherent, reviewable parts. Resolve material blockers while continuing work that does not depend on them. If no plan is available, locate the referenced artifact or ask for its location rather than inventing one.

Implement the requested scope, following nearby patterns and keeping changes focused. The user's request controls how many plan items to complete; do not impose a one-item limit. Update documentation when appropriate.

Follow project rules for tests and validation. Do not run tests unless explicitly requested. Use existing configured static checks when useful and permitted. Extend focused behavioral coverage when warranted, without requiring tests for every change. Repeat or broaden checks only after a change, failure, or unresolved concern justifies it.

Update the plan with completed items, concise outcomes, and discovered work. Mark an item complete only when its acceptance criteria are met; record blocked or partially verified items accurately. Consult [references/summary-examples.md](references/summary-examples.md) only when useful for writing progress notes. Keep changes to project-wide instructions within the user's requested scope. Preserve `PLAN.md` and `SPEC.md` when work finishes unless asked to remove them. Do not stage, commit, or alter Git history without explicit authorization.

## Addressing review feedback

Before editing, identify each outstanding comment, its target file/function, and the requested outcome. Carry applicable feedback across all named locations; fixing one similar function does not resolve comments on another.

If the user repeats a structural instruction, compare the current implementation with the original code. Remove your conflicting restructuring and apply the requested change directly. Do not respond with another variation of the rejected design.

Before claiming completion, review the actual diff against each comment. For a request to preserve existing code, confirm that its branches, ordering, and formatting remain recognizable and that added code serves the requested behavior. Passing lint checks does not establish that the comment was addressed.

Before reporting completion, review the final diff and acceptance criteria against good-micro-management; accurately record incomplete work and verification limits.

Report changes, verification, and remaining risks or blockers.
